## hello
## Context

This blogpost is written as a part of the tool created for a school project from the third year students at the SAE institute of Geneva. This project consist of a game that is made inside of a custom game engine called the Neko Engine. For this project, us, the second year students were asked to create tools to help them in their project. 

For this reason, I created an UI Exporter tool in order to allow them to easily export the UI elements from their Unity prototype into the custom engine.

## The UI Exporter Tool

### What is the goal of this tool

This tool is used to export the UI elements from a Unity scene into a file in a JSON format. It will use the canvas present in the scene and associated with the UI elements to serialize and export the needed data from each object into the JSON format and save them into a custom file. It needs to be effective and easily used aswell as easily editable in case new functionality have to be added or changes to be done.

### General look of the tool

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1610481235569/kxAQBJcaa.png)

![notice gif.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1610481530312/rF_Mzruf9.gif)

### Tool datas
Before looking at how the tool actually work, it is important to know what is needed inside of it in order to register the differents datas needed from the UI gameObjects. To do this, the tool contains multiple struct and class in order to define the components and their variables. The tool contains at least three of them:

- The `UIObject` class

```
[Serializable]
    public class UIObject 
    {
        public bool active;
        public int ID;
        public string name;

        public RectTransform rectTransform = new RectTransform();
        public ImageComponent imageComponent;
        public ButtonComponent buttonComponent;
        public SliderComponent sliderComponent;
        public TextMeshComponent textMeshComponent;

        public int parentID;
    }
``` 
This class is the definition of the UI gameObjects present in the scene and regroup all their requested datas and components.


- The `ObjectCollection` class

```
[Serializable]
    public class ObjectsCollection
        public List<UIObject> Elements;
    }
    ObjectsCollection collection;
``` 
This class contain the `Elements` list wich will regroup every `UIObjects` and is needed for the JSON final serialization.

- The `RectTransform` class
```
 [Serializable]
    public class RectTransform
    {
        public float[] align = new float[4] ;

        public Vector3 position;
      
        public Vector2 anchorsMin;
        public Vector2 anchorsMax;

        public Vector2 pivot;

        public Vector3 rotation;

        public Vector3 localScale;

        public void Fill(GameObject uiObject)
        {
            UnityEngine.RectTransform rectTransform = uiObject.gameObject.GetComponent<UnityEngine.RectTransform>();
            align = new float[] { rectTransform.rect.xMin, rectTransform.rect.xMax, rectTransform.rect.yMin, rectTransform.rect.yMax };
            anchorsMin = rectTransform.anchorMin;
            anchorsMin = rectTransform.anchorMax;
            pivot = rectTransform.pivot;
            rotation = rectTransform.transform.localEulerAngles;
            localScale = rectTransform.localScale;
            position = uiObject.transform.localPosition;
        }
    }
``` 
The `RectTransform` class is the only component class that is present in all the UI gameObjects wich is why it is needed. You can see that it contains the different variables that are requested from this component as well as the `Fill()` method wich is defined here (and need to be defined for every new component) and used to accordingly fill the class with the correct datas from the gameObjects.

Other components can be defined and added by creating other classes or struct the same way. For this project i had to define 4 more of them.








### How does it work

The tool work by executing 4 different steps :


**1. `ExportUI()`**

This is the method called when the user press one of the two buttons and it will start the process of exporting the UI. The method will take a ` Canvas` and start calling the `BrowseHierarchy()` method on it to begin the serialization process.
 
```
 public void ExportUI(Canvas canvas)
    {
        collection = new ObjectsCollection();
        collection.Elements = new List<UIObject>();
        if(canvas == null)
        {
            Debug.Log("no canvas selected or found");
        }
        if (canvas != null)
        {
            BrowseObjectaHierarchy(canvas.transform.gameObject);
        }
    }
``` 
Here, it also initialize the `collection` and its `Element` list

**2.** `BrowseObjectHierarchy()`

This method is called on each `Canvas` and their childrens and works in a recursive way in order to be called on every object inside of every childrens that are part of the `Canvas` until there is no more. The method will then call `RegisterData()` on each of these objects to serlalize their data into the different components and into the final `collection`.

```
 void BrowseObjectaHierarchy(GameObject uiObject) 
    {
        List<GameObject> objectChilds = new List<GameObject>();
        if (uiObject.transform.childCount != 0)
        {
            for (int i = 0; i < uiObject.transform.childCount; i++)
            {
                objectChilds.Add(uiObject.transform.GetChild(i).gameObject);
            }
            foreach(GameObject UiObjectChildren in objectChilds)
            {
                RegisterData(UiObjectChildren);
                BrowseObjectaHierarchy(UiObjectChildren);
            }
        }
    }
``` 
**3.** `RegisterData()`

`RegisterData()`, wich is called each time in the `BrowseObjectHierarchy()`, is used to register the data from the gameObject and its components into the components data structure. The method instantiate a new `UIObject objectDefinition` each time and then fill it with the gameObject data. It will register the general data and then look for the components if they are present in the gameObject. If it's the case, it will call the `Fill()` method present in the definition of each component to fill them.

```
void RegisterData(GameObject obj) // register the informations about the UI object and add the definition into the list
    {
        UIObject objectDefinition = new UIObject();

        objectDefinition.active = obj.activeSelf;
        objectDefinition.name = obj.name;
        objectDefinition.ID = obj.GetInstanceID();

        objectDefinition.rectTransform.Fill(obj);

       if(obj.GetComponent<UnityEngine.UI.Image>() != null)
        {
            objectDefinition.imageComponent.Fill(obj);   
        }

       if(obj.GetComponent<UnityEngine.UI.Button>() != null)
        {
            objectDefinition.buttonComponent.Fill(obj);
        }

        if (obj.GetComponent<UnityEngine.UI.Slider>() != null)
        {
            objectDefinition.sliderComponent.Fill(obj);
        }

       if(obj.GetComponent<TextMeshProUGUI>() != null)
        {
            objectDefinition.textMeshComponent.Fill(obj);
        }

        if (obj.transform.parent != null)
        {
            objectDefinition.parentID = obj.transform.parent.GetInstanceID();
        }

         collection.Elements.Add(objectDefinition);
    }
``` 
Finally, the `objectDefinition` is added inside the collection that will later be used to create the JSON file.

**4. `SaveJson()`
**

The `SaveJson()` method is the final step and it's a this point that the data are serialized into the JSON format. Since the previous step have been done on each gameObjects, the `collection` is now filled with every `objectDefinition` object. `SaveJson()` will then serialize the entire collection into a string and write it into the previously selected folder using the `savePath` string.

```
void SaveJson(string savePath) 
    {
        UIJsonFormat = JsonUtility.ToJson(collection, true);
       
        if (!System.IO.File.Exists(savePath))
        {
            System.IO.File.WriteAllText(savePath, UIJsonFormat);
        }
    }
``` 

After this, the UI is correctly serialized into a file and can be opened in the correspondent folder for its later use in the custom engine to recreate the user interface as close as possible from how it was in the Unity prototype if enough data where collected.

## Conslusion
In the end, i am happy with my tool. It works well and i managed to implement what was requested. I would improve the second functionality wich is to export every canvas by managing to have acces to a list of them in order to know exactly how many of them there is and what will be exported.
