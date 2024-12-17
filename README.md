# Minimal Example: `camera-tools .fitToSphere` on Page Load Not Working

Hello,

In my project, I want to dynamically adjust the camera settings so that my object - in this example, the spaceship — fits into the viewport of my canvas. This should work both on **window resize** and **page load**. The pan/dollyTo of the camera works as intended when resizing the window. However, it does not work on the initial page load: the camera zooms into the center of the ship, so the view ends up *inside* the ship. Upon resizing the window, the camera moves out to fit the ship into view (as intended). 

After some investigation, I believe I have identified the issue. When implementing the same behavior using "vanilla" Three.js, I encountered a similar problem. This was resolved by calling `renderer.render` on the scene **prior to calling `.fitToSphere`/`fitObject`**.

**Does anyone have an idea how to resolve this issue in Threlte?**  
I have tried various approaches, such as placing the `fitToSphere` call into a separate task using the `afterRenderStage`, as outlined in the [Threlte documentation](https://threlte.xyz/docs/learn/basics/scheduling-tasks)—all to no avail.

Most of the relevant code is in `Scene.svelte`.

## Instructions

1. Clone this repository.
2. Install dependencies: `npm i`
3. Start the server: `npm run dev`  
   Open the page in your browser.
4. **Observe:** The camera is inside the spaceship. This is the issue I am trying to resolve.
5. Now **resize the browser window**: The camera moves to display the entire spaceship. This is the behavior I would like to achieve in Step 4.

---

### Side Note

I am fairly new to Threlte/Three.js (and front-end development in general). I would greatly appreciate feedback on any bad practices found in this example.

Thanks a lot!

