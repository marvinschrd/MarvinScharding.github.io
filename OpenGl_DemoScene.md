## Context

This blogpost is written as a part of my computer graphics course at the SAE Institute of Geneva. We had the task to create a demo scene to show different implementations of computer graphics technics using the OpenGL graphic API. I created my scene by expanding a base engine from Elias Farhan

## Used libraries

* OpenGL library
    
* Glad
    
* SDL2
    
* Glm
    
* Gli
    
* Assimp
    
* Stb
    
* Imgui
    

## Ressources

In order to work on this demo scene, i used the courses and teaching provided by my school and my teachers Elias Farhan and Frederic Dubouchet aswell as the excellent tutorials from [LearnOpenGL.com](https://learnopengl.com/Introduction).

## The scene

My scene consist of a fantasy/forest setup where a few different models are placed in the same environment and all rendered using the same different techniques. The camera move to different location to show the scene from different angles. Every models are showcasing the same rendering technics so the camera doesn't need to look at each of them for particular reason.

# Layout

The layout consist horizontaly of a hand statue holding a rotating fire sphere creating a point light, a circular rock pedestal with two baby dragon eggs and a flying insect creature, three old trees and a rock and pieces of grass over the ground.

![layout 0.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626778254836/GjtdXIE9B.png align="left")

## Implementations

### Model loading

The models are all .obj files loaded with their .mtl material files. They are loaded using the assimp library for the models and stb for the textures.

The assimp api exctract the informations needed from the models by iterating through each mesh. They are then loaded on the gpu and binded with their respectives textures for the drawing process. Assimp also allow us to directly calculate the tangent that will later be used in the normal mapping process. Even tough assimp is able to load much more type of files than .obj, i still only used them because i could manage their .mtl files and learn at the same time.

### Lighting

The ligthing technique applied to the models in the shaders is the Blinn phong illumination. It use ambient, diffuse and specular values to simulate lighting. It is not physically realistic but it gives a proper result for a simple demo scene.

* Directional light I added two light with two different types in my scene. The first one is a directional light coming from the Z axis and slightly gowing down. It illuminate from a single direction the integrality of the scene.
    
* Point light The second light is a point light, coming from the position of the rotating sphere. It illuminate the objects around him and not the entirety of the scene. Since point lights are omnidirectionnal, the light has multiple directions that need to be calculated. I also calculate an attenuation value using quadratic and linear value in order to define a ligthing distance around the point light.
    

```plaintext
vec3 CalculatePointLight(vec3 normal, vec3 out_position, vec3 viewDir, float shadow)
{
    vec3 color = texture(texture_diffuse1,out_tex).rgb;
     // Compute ambient light.
    vec3 ambient = ambient_strengh * color * light_color2;

    vec3 light_direction = normalize(tangentLightPos - tangentFragPos);
    float diff = max(dot(normal, light_direction), 0.0);
    vec3 diffuse = diff * light_color2;


    vec3 reflection_direction = reflect(-light_direction,normal);
    vec3 halfWayDir = normalize(light_direction + viewDir);
    float spec = pow(max(dot(normal, halfWayDir), 0.0), specular_pow);
    vec3 specular = specular_strength * spec * light_color2;

    // atenuation
    float distance    = length(pointLight_Position - out_position);
    float attenuation = 1.0 / (constant + linear * distance + 
    		    quadratic * (distance * distance));

    ambient  *= attenuation;  
    diffuse   *= attenuation;
    specular *= attenuation; 

    //emission light
    vec3 emission = texture(texture_emission1, out_tex).rgb;

    vec3 lighting = (ambient +(1.0-shadow) * (diffuse + specular) + emission) * color;

    vec3 result_specular =
       specular * texture(texture_specular1, out_tex).r;

     lighting +=result_specular;

    return lighting;
}
```

Since both light needs to be caclulated to detemine the final ligthing of objects, i had to calculate them separately and then add their respective result to the final Frag colors.

### Normal mapping

Normal mapping allow the models to use a normal map texture on top of the diffuse and specular one. The positions are calculated in tangent space instead and the normals are extracted from the normal map. This allow for more detailed three dimentional lighting without having overcomplicated surfaces. A TBN matrice is used to transform each position into tangent space in the shaders. As previously said, tangent are extracted by assimp when models are loaded and stored in the gpu. Bitangent can also be exctracted directly but i am calculating them directly in the vertex shader.

This is the normal map of the baby dragon egg shell

![eggNormal.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626819965606/IXlnWmwu5.png align="left")

Normal map on the trees for exemple

![normal mapping 0.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626820025102/Vcyhn97_i.png align="left")

### Shadow mapping

Then next technique is shadow mapping. Shadow mapping compute shadow regarding the position or direction of the lights in order to further simulate pseudo realistic lighting.

In my scene, even tough there are two different types of lights, i only computed and used directional shadows because it is easier and i lacked time to debug my attempt at point shadows.

Shadow mapping use a frambuffer with a depth map to calculate the depth of the scene from a position and in light space as if we were looking from the light perspective to calculate if a fragment is or not in the shadow. For that, the scene has to be rendered at least two times. First to fill the depthmap, and then to actually render everything with the shadows.

The depth map used to compute shadows

![depthMap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626820040334/7aUw6Yrdk.png align="left")

Here you can see the shadows.

![shadow2 0.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626820077839/dTXJ8yrXh.png align="left")

They are calculated in the fragment shader by using the light space matrix and a bias is then added to prevent shadow acnee. PCF is then applied to soften the edges of the shadows by sampling from nearby fragments.

The shadow pass :

```plaintext
glm::mat4 light_Projection, light_View;
		glm::mat4 lightSpaceMatrix;
		float near_plane = 1.0f, far_plane = 100.0f;
		light_Projection = glm::ortho(-50.0f, 50.0f, -50.0f, 50.0f, near_plane, far_plane);
		light_View = glm::lookAt(light_position, light_position - light_Dir, glm::vec3(0.0, 1.0, 0.0));
		lightSpaceMatrix = light_Projection * light_View;

		//Shadow Pass
		simpleDepthShaders_->Use();
		simpleDepthShaders_->SetMat4("lightSpaceMatrix", lightSpaceMatrix);

		glViewport(0, 0, shadowSizeHeight_, shadowSizeWidth_);
		glBindFramebuffer(GL_FRAMEBUFFER, depthMapFBO);
		glClear(GL_DEPTH_BUFFER_BIT);
		//glActiveTexture(GL_TEXTURE0);
		glCullFace(GL_FRONT);
		RenderScene(simpleDepthShaders_);
		glCullFace(GL_BACK);
		glBindFramebuffer(GL_FRAMEBUFFER, 0);
```

The shadow caclulation in the fragment shader :

```plaintext
float ShadowCalculations(vec4 fragPosLightSpace)
{
    float bias = max(0.001f * (1.0 - dot(out_normal, tangentLightDirection)), 0.0005f);
    vec3 projCoords = fragPosLightSpace.xyz / fragPosLightSpace.w;
    // transform to [0,1] range
    projCoords = projCoords * 0.5 + 0.5;
    // get closest depth value from light's perspective (using [0,1] range fragPosLight as coords)
    float closestDepth = texture(shadowMap, projCoords.xy).r; 
    // get depth of current fragment from light's perspective
    float currentDepth = projCoords.z;
    // check whether current frag pos is in shadow

    //PCF
    float shadow = 0.0;
    vec2 texelSize = 1.0 / textureSize(shadowMap, 0);
    for(int x = -1; x <= 1; ++x)
    {
        for(int y = -1; y <= 1; ++y)
        {
            float pcfDepth = texture(shadowMap, projCoords.xy + vec2(x, y) * texelSize).r; 
            shadow += currentDepth - bias > pcfDepth  ? 1.0 : 0.0;        
        }    
    }
    shadow /= 9.0;

    if(projCoords.z > 1.0)
        shadow = 0.0;

    return shadow;
}
```

### HDR and gamma correction

After the scene and the shadows are rendered, a post processing step is applied to use HDR and gamma correction. HDR allow us to use brighter lights without losing details from objects or without them becoming instantly white. For that, a postprecessing framebuffer is used but with floating points. This floating point framebuffer allow us to have greater lights with a larger range of colors in HDR with 16bits of colors instead of the classical 8. But since our screen are not working in HDR, we have to go back to LDR before rendering the final frame. This is done by using Reinhard tone mapping wich will bring our frame back to LDR with the benefits of the HDR colors.

```plaintext
#version 450 core
out vec4 FragColor;

in vec2 TexCoords;

uniform sampler2D hdrBuffer;


void main()
{             
    const float gamma = 2.2;
    vec3 hdrColor = texture(hdrBuffer, TexCoords).rgb;
   
    // reinhard
    vec3 result = hdrColor / (hdrColor + vec3(1.0));
            
    result = pow(result, vec3(1.0 / gamma));
    FragColor = vec4(result, 1.0);
    
}
```

The scene before final hdr and gamma correction.

![hdrGammaCoreection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626820106259/TO-bbIaEZ.png align="left")

The scene after the correction

![hdrGammaCorrectionAfter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626820158048/2teBjqVAQ.png align="left")

## A few optimisations

Optimisation are very important in computer graphics to maintain the framerate to a descent level. In this scene, there isn't much optimisations but it is something that really needs to be done.

### Load textures once

The first and small optimisation that is used is preventing te reloading of similare texture to the gpu. This way, if a different models or meshes are using the same texture, it will only be loaded once and used accordingly.

```plaintext
std::vector<TextureAssimp> Model::LoadMaterialTextures(aiMaterial* material, aiTextureType textureType, std::string typeName)
	{
		std::vector<TextureAssimp> textures;
		const size_t textureCount = material->GetTextureCount(textureType);
		for(unsigned int i = 0; i < textureCount; ++i)
		{
			aiString string;
			material->GetTexture(textureType, i, &string);
			bool skip = false;
			const size_t sizeOfTextureLoaded = texturesLoaded_.size();
			for(unsigned int j = 0; j < texturesLoaded_.size(); ++j)
			{
				if (std::strcmp(texturesLoaded_[j].path.data(), string.C_Str()) == 0)
				{
					textures.push_back(texturesLoaded_[j]);
					skip = true;
					break;
				}
				assert(j < sizeOfTextureLoaded);
			}
			if(!skip)
			{
				TextureAssimp texture;
				
				texture.ID = LoadTextureFromFile(string.C_Str(), textureDirectory, textureType);
				texture.type = typeName;
				texture.path = string.C_Str();
				textures.push_back(texture);
				texturesLoaded_.push_back(texture);
			}
		}
		return textures;
	}
```

### Instancing

The next optimisation is the use of instancing for the rendering of similar models. Instancing is used to reduce the number of draw calls and therefore the number of communication between the cpu and the gpu. In this case, i used it to instanciate the grass models randomly in the scene since it was the only model i was using more than once. With instancing, all of the grass models are drawn in a single draw call because all of their datas have already been loaded to the gpu. It is a vital optimisation for complex scenes where the same models could be used thousands of time.

### Back face culling

This is rather small and easy because it can be done by openGl itself. But it is usefull because with this optimisations, only the front faces of the models are rendered, the ones facing the camera instead of the whole object even if we wouldn't notice it.

### Further optimisations i could have done

More optimisations could have be used in this scene but i couldn't had them or didn't find enough time for.

* Frustum culling
    

Frustum culling would have allowed me to only render the objects that are inside the frustum of the camera. It is very usefull and can drastically reduce the number of objects rendered at each frame. If an object is totally out of the frustum, he is not rendered.

* Deffered rendering
    

Deffered rendering is used to calculate the ligthing and shading of fragment only once. It uses a G-buffer (geometry buffer) to store different data on multiple render target. The position, the normal and the colors are stored in these textures and the lighting computing is done only at the end using the informations from the G-buffer. If the devices are not too old, it can drastically increase the framerate.

## Conclusion

I am very happy to have been introduced to the world of graphic programming because it is very interesting and exciting. I really like to visually see what i am doing and i liked being able to render ideas by myself even if it is hard. I would have liked to have more time on my side to add further techniques such as the optimisations i talked about, or the bloom effect for exemple but in the end i was able to create a scene that i liked even if it is not a very advanced one. I will further upgrade it because i had fun in what i had the time to try.
