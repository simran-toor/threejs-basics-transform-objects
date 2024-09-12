# 05 TRANSFORM OBJECTS NOTES 

## PROPERTIES TO TRANSFORM OBJECTS
    1. Position - move object
    2. Scale - resize object
    3. Rotation - rotate object
    4. Quaternion - also to rotate object 
* All classes that are inherited from Object3D possess the above properties
    *    e.g. PerspectiveCamera or Mesh
    *    can see inheritance in Three.js docs (Object3D -> Camera -> PerspectiveCamera)
* These properties are compiled in matrices

## MOVE OBJECTS
* using position which has 3 properties: x, y, z
    1. Position
        * can move camera 
        `camera.position.z = 3`
        * can use position property of mesh (b/c inherited from Object3D)
        `mesh.position.y = 1`
        * Position is not just an object but an instance of Vector3 (which is a class used to position things in a space)
            *has more than just x, y, z 
            * lots of methods available in docs
                * length of vector
                `console.log(mesh.position.length())`
                * distance from another vector
                `console.log(mesh.position.distanceTo(camera.position))`
                * can normalise its values
                `mesh.position.normalize()`
                * change values of x, y, z together
                `mesh.position.set(0.7, -0.6, 1)`

## AXIS HELPER
* Use Three.js AxesHelper to know where each axis is orientated  
* Displays 3 lines corresponding to x, y, z axis 
    ```
    const axesHelper = new THREE.AxesHelper()
    scene.add(axesHelper)
    ```

## SCALE
* Also vector3
* By default x, y, z are equal to 1 
```
mesh.scale.x = 2
mesh.scale.y = 0.25
mesh.scale.z = 0.5
```

## ROTATE OBJECTS 
* Can use rotation or quaternion properties (Three.js supports both and updating one automatically updates the other)
### ROTATION
* Rotation property has x, y, z values
* Euler angles, 3 axes of rotational values (yaw, pitch, roll)
* Uses Radians 
    * half rotation is Pi 
    * native JS use `Math.PI`
* Comment the scale and add an eighth of a complete rotation in both x and y axes
    ```
    mesh.rotation.x = Math.PI * 0.25
    mesh.rotation.y = Math.PI * 0.25
    ```
* Advantages 
    * Easy to understand
* Disadvantages
    * combing rotations produces strange results 
    * when you rotate x axis, the other axes' orientation also changes 
    * rotation applies in x, y then z order
    * causes issues like 'gimbal lock' where one axis has no more effect b/c of previous one
    * can change order by using the `reorder(...)` method `object.rotation.reorder('YXZ')`
* While Euler is easier to understand, rotation order causes too many issues
* B/c of this most engines and 3D softwares use Quaternion instead
### QUATERNION 
* Also expresses a rotation but in a more mathematical way
* Covered in a different lesson 

## LOOK AT THIS! 
* Object3D instances have method called `lookAt(...)`, where you can make an object look at something 
* Object automatically roates its -z axis towards specified target 
* Example uses: 
    * rotate camera towards object
    * orientate cannon to face an enemy
    * move characters eyes to an object 
* The parameter is the target and must be a Vector3 
    ```
    camera.lookAt(new THREE.Vector3(0, -1, 0))
    ```

## COMBINING TRANSFORMATIONS 
* Can combine position, rotation, and the scale in any order,
* Combine all the transformations tried before: 
    ```
    mesh.position.x = 0.7
    mesh.position.y = - 0.6
    mesh.position.z = 1
    mesh.scale.x = 2
    mesh.scale.y = 0.25
    mesh.scale.z = 0.5
    mesh.rotation.x = Math.PI * 0.25
    mesh.rotation.y = Math.PI * 0.25
    ```
## SCENE GRAPH 
* Used to group things 
* Example: house with walls, doors, windows, roof etc. If house is too small instead of resizing each object you can group them instead and update their scale and position
* Good alternative is to group all objects into a container and scale the container
* Group class: 
    * instantiate a `Group` and add it to scene 
    * when you want to create new obkect, you can add it to the `Group` using the `add(...)` method rather than adding it directly to the scene 
    * `Group` class inherits from `Object3D` class, so it has access to properties and methods like position, scale, roatation, quaternion, and `lookAt`
* comment out the `lookAt(...)` call and create 3 cubes and add them to a `Group`. Then apply transformations on the `group`
    ```
    /**
    * Objects
    */
    const group = new THREE.Group()
    group.scale.y = 2
    group.rotation.y = 0.2
    scene.add(group)

    const cube1 = new THREE.Mesh(
        new THREE.BoxGeometry(1, 1, 1),
        new THREE.MeshBasicMaterial({ color: 0xff0000 })
    )
    cube1.position.x = - 1.5
    group.add(cube1)

    const cube2 = new THREE.Mesh(
        new THREE.BoxGeometry(1, 1, 1),
        new THREE.MeshBasicMaterial({ color: 0xff0000 })
    )
    cube2.position.x = 0
    group.add(cube2)

    const cube3 = new THREE.Mesh(
        new THREE.BoxGeometry(1, 1, 1),
        new THREE.MeshBasicMaterial({ color: 0xff0000 })
    )
    cube3.position.x = 1.5
    group.add(cube3)
    ```


