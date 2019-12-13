## Light Camera Action


 > ### Building a Simple 3D scene with Three JS, StatsJS, Dat.GUI & Orbit Control as React component


  

<iframe src="https://codesandbox.io/embed/relaxed-fog-opzcc?fontsize=14" title="Three Template" style="width:100%; height:500px; border:0;margin: 10px; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>






## Learnings

- Using Stats to show FPS & Memory usage
- Using DAT.GUI to control Scene
- Animating Stuffs 
- Loading & using textures 
- Handling Keyboard Key Press
- Setting Panorama Background
- Using Helpers classes to assist with Axis & Grid 


--



 ## Final Source Code

    import React, { Component } from "react";
    import * as THREE from "three";
    import OrbitControls from "three-orbitcontrols";
    import Stats from "stats-js";
    import * as dat from "dat.gui";
    //Scene
    var cube, camera; //model
    var orbit; // orbit of light
    var bulbLight; //orbitting light
    var bulbMat;

    //Helper
    const AXIS_SIZE = 50;
    const GRID_SIZE = 100;
    const GRID_DIVISION = 10;

    //Incremental Animation
    var step = 0;

    var then;
    var incrementalAnimate = true;
    const MIN_INTERVAL = 100;
    const ITERATION_COUNT = 5000;

    var options = {
        angularSpeed: 0.01,
        velx: 0.1,
        vely: 0.1,
        shouldAnimate: true,
        field: "Enter a Text",
        color: "#ffae23", // CSS string

        cameraControl: {
        speed: 0.05
        },
        stop: () => {
        this.velx = 0.1;
        this.vely = 0.1;
        this.angularSpeed = 0.0;
        this.shouldAnimate = false;
        },
        reset: function() {
        this.velx = 0.1;
        this.vely = 0.1;
        this.angularSpeed = 0.01;
        this.shouldAnimate = true;
        cube.scale.x = 1;
        cube.scale.y = 1;
        cube.scale.z = 1;
        cube.position.x = 0;
        cube.position.y = 10;
        cube.position.z = 0;
        cube.material.wireframe = true;

        camera.position.x = -30;
        camera.position.y = 15;
        camera.position.z = -30;
        }
    };

    //fps stats
    var statsFPS = new Stats();
    statsFPS.domElement.style.cssText = "position:absolute;top:3px;left:3px;";
    statsFPS.showPanel(0); // 0: fps,

    //memory stats
    var statsMemory = new Stats();
    statsMemory.showPanel(2); //2: mb, 1: ms, 3+: custom
    statsMemory.domElement.style.cssText = "position:absolute;top:3px;left:84px;";

    //Textures
    const BACKGROUND =
        "https://images.pexels.com/photos/290595/pexels-photo-290595.jpeg";
    const WOOD = "https://images.pexels.com/photos/172292/pexels-photo-172292.jpeg";

    class ThreeScene extends Component {
        constructor(props) {
        super(props);
        this.state = { useWireFrame: true };
        }
        componentDidMount() {
        //add stats for FPS and Memory usage
        document.body.appendChild(statsFPS.dom);
        document.body.appendChild(statsMemory.dom);

        //Add Light Camera
        this.addScene();

        this.addModel();

        // Add Events
        window.addEventListener("resize", this.onWindowResize, false);
        document.addEventListener("keydown", this.onDocumentKeyDown, false);
        document.addEventListener("keyup", this.onDocumentKeyUp, false);

        //ACTION
        this.renderScene();
        //start animation
        this.start();
        }

        addDatGUI = () => {
        this.gui = new dat.GUI({ name: "My GUI" });

        var cam = this.gui.addFolder("Light & Camera");
        cam
            .add(options.cameraControl, "speed", 0.01, 0.1)
            .name("ω")
            .listen();
        cam.add(camera.position, "y", -100, 100).listen();
        cam.open();

        var velocity = this.gui.addFolder("Velocity");

        velocity
            .add(options, "angularSpeed", -0.2, 0.2)
            .name("ω")
            .listen();
        velocity
            .add(options, "velx", -5, 5)
            .name("Vx")
            .listen();
        velocity
            .add(options, "vely", -5, 5)
            .name("Vy")
            .listen();
        velocity.open();

        var fields = this.gui.addFolder("Field");
        fields.add(options, "field");
        fields.addColor(options, "color");

        fields.open();

        var box = this.gui.addFolder("Cube");

        box
            .add(cube.scale, "x", 0, 3)
            .name("Width")
            .listen();
        box
            .add(cube.scale, "y", 0, 3)
            .name("Height")
            .listen();
        box
            .add(cube.scale, "z", 0, 3)
            .name("Length")
            .listen();
        box.add(cube.material, "wireframe").listen();
        box.add(options, "shouldAnimate").listen();

        box.open();

        this.gui.add(options, "stop");
        this.gui.add(options, "reset");
        };

        incrementalAnimation = () => {
        let now = performance.now();
        if (then) {
            if (now - then > MIN_INTERVAL) {
            if (step < ITERATION_COUNT) {
                this.drawNextPoint();
                then = now;
                step++;
            } else {
                incrementalAnimate = false;
            }
            }
        } else {
            then = now;
        }
        };
        drawNextPoint = () => {
        //incremental update model
        };

        start = () => {
        if (!this.frameId) {
            this.frameId = requestAnimationFrame(this.animate);
        }
        };
        stop = () => {
        cancelAnimationFrame(this.frameId);
        };

        animate = () => {
        // Spin Point Light
        if (orbit) orbit.rotation.y += options.cameraControl.speed;
        if (options.shouldAnimate) {
            if (cube) cube.rotation.x += options.angularSpeed;
            if (cube) cube.rotation.z += options.angularSpeed;
        }

        if (incrementalAnimate) {
            this.incrementalAnimation();
        }

        //Dynamic lighting
        let time = Date.now() * 0.00005;
        let h = ((360 * (1.0 + time)) % 360) / 360;
        bulbLight.color.setHSL(h, 0.5, 0.5);
        bulbMat.emissive.setHSL(h, 0.5, 0.5);

        //update stats
        statsFPS.update();
        statsMemory.update();

        //redraw scene
        this.renderScene();

        this.frameId = window.requestAnimationFrame(this.animate);
        };

        renderScene = () => {
        if (this.renderer) this.renderer.render(this.scene, camera);
        };

        componentWillUnmount() {
        this.stop();
        document.removeEventListener("resize", this.onWindowResize, false);
        document.removeEventListener("keydown", this.onDocumentKeyDown, false);
        document.removeEventListener("keyup", this.onDocumentKeyUp, false);
        this.mount.removeChild(this.renderer.domElement);
        }

        onWindowResize = () => {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        };

        onDocumentKeyUp = event => {
        options.shouldAnimate = true;
        };

        onDocumentKeyDown = event => {
        options.shouldAnimate = false;

        var keyCode = event.which;
        switch (keyCode) {
            case 87: {
            cube.position.z += options.vely; //W
            break;
            }
            case 83: {
            cube.position.z -= options.vely; //S
            break;
            }
            case 65: {
            cube.position.x -= options.velx; //A
            break;
            }
            case 68: {
            cube.position.x += options.velx; //D
            break;
            }
            case 32: {
            cube.position.set(0, 10, 0); //SPACE
            break;
            }
            default: {
            break;
            }
        }
        };

        addModel = () => {
        //Load Wood Texture
        var loader = new THREE.TextureLoader();
        loader.load(
            WOOD,
            //use texture as material
            texture => {
            var woodMaterial = new THREE.MeshPhongMaterial({
                map: texture
            });

            var cubegeometry = new THREE.BoxGeometry(5, 5, 5, 10, 10, 10);
            cube = new THREE.Mesh(cubegeometry, woodMaterial);
            cube.position.y = 10;

            this.scene.add(cube);

            // Add DAT GUI after adding model
            this.addDatGUI();
            }
        );
        };

        /**
        * Boilder plate to add LIGHTS,Renderer, Axis, Grid,
        */
        addScene = () => {
        const width = this.mount.clientWidth;
        const height = this.mount.clientHeight;
        this.scene = new THREE.Scene();

        // Add Renderer
        this.renderer = new THREE.WebGLRenderer({ antialias: true });
        this.renderer.setClearColor("#263238");
        this.renderer.setSize(width, height);
        this.mount.appendChild(this.renderer.domElement);

        // Add Camera
        camera = new THREE.PerspectiveCamera(48, width / height, 0.1, 1000);
        camera.position.x = -30;
        camera.position.y = 15;
        camera.position.z = -30;

        // Add Background
        let panaromaTexture = new THREE.TextureLoader().load(BACKGROUND);
        panaromaTexture.magFilter = THREE.LinearFilter;
        panaromaTexture.minFilter = THREE.LinearFilter;

        //create material using texture
        const shader = THREE.ShaderLib.equirect;
        const bgMaterial = new THREE.ShaderMaterial({
            fragmentShader: shader.fragmentShader,
            vertexShader: shader.vertexShader,
            uniforms: shader.uniforms,
            depthWrite: false,
            side: THREE.BackSide
        });

        bgMaterial.uniforms.tEquirect.value = panaromaTexture;
        const bgBox = new THREE.BoxBufferGeometry(700, 700, 700);
        let bgMesh = new THREE.Mesh(bgBox, bgMaterial);
        this.scene.add(bgMesh);

        //Add controls
        const controls = new OrbitControls(camera, this.renderer.domElement);
        controls.enableDamping = true;
        controls.dampingFactor = 0.25;
        controls.enableZoom = true;
        controls.autoRotate = true;
        controls.keys = {
            LEFT: 37, //left arrow
            UP: 38, // up arrow
            RIGHT: 39, // right arrow
            BOTTOM: 40 // down arrow
        };

        //-------------HELPER------------------
        // Add Grid
        let gridXZ = new THREE.GridHelper(
            GRID_SIZE,
            GRID_DIVISION,
            0x18ffff, //center line color
            0x42a5f5 //grid color,
        );
        this.scene.add(gridXZ);

        // Add AXiS
        let axesHelper = new THREE.AxesHelper(AXIS_SIZE);
        axesHelper.position.y = 10;
        this.scene.add(axesHelper);

        //-------------LIGHTS------------------
        //top light
        let topLight = new THREE.PointLight(0xffffff, 1, 0); //color ,intensity, distance
        topLight.position.set(0, 100, 0);
        this.scene.add(topLight);

        //bottom light
        let bottomLight = new THREE.PointLight(0xffffff, 1, 0); //color ,intensity, distance
        bottomLight.position.set(0, -100, 0);
        this.scene.add(bottomLight);

        // Spining point light
        orbit = new THREE.Group();
        var bulbGeometry = new THREE.SphereBufferGeometry(1.5, 8, 32);
        bulbLight = new THREE.PointLight(0xffee88, 2.5, 0); //color ,intensity,distance
        bulbMat = new THREE.MeshStandardMaterial({
            emissive: 0xffee88,
            emissiveIntensity: 1,
            color: 0x000000
        });
        bulbLight.add(new THREE.Mesh(bulbGeometry, bulbMat));
        bulbLight.position.set(50, 0, 0);
        bulbLight.castShadow = true;
        orbit.add(bulbLight);
        this.scene.add(orbit);

        // background Sun light
        var sunLight = new THREE.SpotLight(0xffffff, 0.1, 0, Math.PI / 2);
        sunLight.position.set(0, 0, 2000);
        sunLight.castShadow = true;
        sunLight.shadow.bias = -0.0002;
        sunLight.shadow.camera.far = 4000;
        sunLight.shadow.camera.near = 750;
        sunLight.shadow.camera.fov = 30;
        this.scene.add(sunLight);
        };

        render() {
            return (
            <div
                style={{ width: window.innerWidth, height: window.innerHeight }}
                ref={mount => {
                this.mount = mount;
                }}
            />
            );
        }
        }
        export default ThreeScene;
