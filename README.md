# JSONWORLD
a web experience


## What is it?
jsonworld is for anyone who wants to create 3D interactive javascript experiences. It makes the process easier by taking the bulk of the javascript coding out of your hands. You don't need to be javascript savy to use this module, but you need some basic knowledge.

At it's core, jsonworld is an abstraction of the 3D tool [**THREEJS**](https://threejs.org) and the animation tool [**ANIMEJS**](https://animejs.com). It's function is to take a json **( Javascript Object Notation )** file full of options and spit out a 3D world based on what you choose. Below is an example of a jsonworld project.

```javascript

    import JSONWorld from "jsonworld";
    
    const tree = {
        "type" : "tree",
        "count" : 100,
        "position" : [ 0, 0, 0 ],
        "color" : "red"
    }

    const cylinder = {
        "type" : "cylinder",
        "count" : 20,
        "vertexAnimation" : "wavy",
        "animation" : "spin-basic 10s ease-in-sine 1s",
        "animationKeyframes": {
            "spin-basic" : "360 20 100 120"
        }
        
    }   
    
    let config = {
        worldObjects : [ tree, cylinder ]
    };

    window.onload = () => {
        let world = JSONWorld(config);
        world.start(config);
    }

```

## Starting A New Project

To start a new jsonworld, you need to create a html ***canvas*** element

```html

<canvas id="world"></canvas>

```

Use an id in order to seperate this canvas element from any other that might be in your html. The default id jsonworld uses is **"world"**, but we can changed the id name in our configuration json file. If a canvas element isn't created, jsonworld will make one for you!

Now we need to write the javascript to get the jsonworld running. Below is all it takes to start:

```javascript
import JSONWorld from "jsonworld";

window.onload = () => {
    
    let world = new JSONWorld({});
    world.start();
    
    console.log( world );
                       
}
```
And wallah! If you refresh your page, you should see a perfect dodecahedron rotating in the middle of the screen. **This is your preloader**. It won't load because we don't have anything in our world to process.  Also, it comes with a light source and mouse movement straight out the package. We created a 3D world with barely any code. But Let's actually add more and make our project shine.

## Building an Object

It's cool to see a 3D object, but it's not all that impressive. The syntax for describing your object is very similar to CSS3, with some minor tweaks. Let's explore creating an *object* in your **worldObjects** and add some attributes.

### Basic Object Attributes

```json

{
    "worldObjects" : [
        {
            "type" : "sphere", //declare what type of object you want to make
            "size" : "10 100 10", //give the object a size by "width heigth depth". You can also use an array [ width, height, depth ].
            "position" : "100 0 0", //set the position of the object by it's axis "x y z". You can also use an array [ x, y, z ]. By default, positioning is set based on the world's origin which is ( 0, 0, 0 ).
            "color" : "red", //set a color for the object. Takes CSS syntax ( "rgb(1, 1, 1 )" ) , literal syntax as you see here or hexidecimals.
            "count" : 5, //this attribute creates clones of root object type. Although you can put as large number of objects on the screen at a time, I recommened between 1 to 1000 at a time for best performance
            "shadow" : true, //a boolean that controls if this object can receive and cast shadows onto the world
        }
    ]
}

```
As you can see, we have 5 spheres lined up along the center of the screen, but something is wrong. There is no depth! They look 2D. Although we turned the shadows on above, **_3D Objects_ require you to use a property called _material_** in order for the object to show any effects. Similar to human skin, you can think of "Material" as the skin of a world object. This will allow us to change the asthetic to differentiate all the items that may populate the screen. 

## Material Attributes

Here is where the [**THREEJS**](https://https://threejs.org/docs/#api/constants/Materials) lingo comes in ( click the link for a deeper explanation for advanced use ). Materials control the appearance of an object. Here are your material options below: 

Type | Description
------------ | -------------
basic | applys a color to the objects skin but **WILL NOT CAST SHADOW**. You can also add textures
normal | applys a preset color for each face, useful for debug when you're creating objects
standard | applys a color, shading, light emission from object and accepts images for textures. **Best for most situations**
phong | similar to standard, but more for creating shiny surfaces like glass or ceramic
lambert | similar to phong, but more for creating dull surfaces like wood or rubber
wireframe | shows each triangle ( face ) using lines that make up your images shape. Great for debugging
toon | similar to standard, but makes your object look more like a cartoon or cel-shaded


