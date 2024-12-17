# Minimal example: camera-tools .fitToSphere on page load not working
Hello,

for a project, I want dynamically set the camera settings such that my object - in this example the spaceship - fits into the viewport of my canvas. This should be done both on window resize as well as upon page load. The pan/dollyTo of the camera works for resizing the window as intended. Unfortunately, it does not work for the initial page load: The camera zooms into the center of the ship, such that one can see the ship from inside. Upon resizing the window the camera moves out to fit the ship into view (as intended). By now, I believe to have figured out what the problem is. Implementing the same in 'vanilla' threejs, I experience a similar issue that is resolved by calling `renderer.render` on the scene, prior to calling `.fitToSphere`.

*Does anyone have an idea how to resolve this issue in Threlte?* I have tried things, like putting the `fitToSphere` call into a seperate Task in to an `afterRenderStage` like outlined in the [docs](https://threlte.xyz/docs/learn/basics/scheduling-tasks) - all to noavail.

## Instructions
1. Clone this repository
2. Install dependencies. I.e.: `npm i`
3. Start server (`npm run dev`) and open page in browser
4. -> the camera is inside of the spaceship. This is the issue i am trying to resolve.
5. Now resize browser window -> camera moves to display entire spaceship. This is the behavior I would like to have in point 4.

Side note: I am fairly new to Threlte/ThreeJS (and front-end development for that matter). So, I am happy to hear about any kind of bad pratices found in this example.

Thanks a lot!
