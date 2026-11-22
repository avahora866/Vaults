### **Rendering in Unity**

Rendering refers to the process of drawing everything visible in the game to the screen. Unity’s rendering system handles how objects are displayed, including how they react to light, textures, and other visual effects.

#### **Key Rendering Concepts:**

1. **Cameras**:  
   * The **Camera** component determines what portion of the scene is visible to the player. You can have multiple cameras in a scene, and Unity will render what each camera sees.  
   * **Field of View (FOV)**: Controls how wide the camera’s view is.  
   * **Clipping Planes**: Determine which objects are visible based on their distance from the camera.  
2. **Shaders**:  
   * Shaders are scripts that tell Unity how to render objects. **Surface shaders** define how light interacts with materials, while **vertex shaders** manipulate how vertices are processed.  
3. **Lighting Models**:  
   * **Phong Lighting**: A shading model that approximates how light interacts with surfaces.  
   * **PBR (Physically-Based Rendering)**: A more advanced lighting model that simulates real-world materials more accurately.  
4. **Render Pipelines**:  
   * Unity uses **Render Pipelines** to define how graphics are rendered. The **Built-in Pipeline** is the default, but you can also use the **Universal Render Pipeline (URP)** or **High Definition Render Pipeline (HDRP)** for better performance and quality.  
5. **Post-Processing**:  
   * Post-processing effects can be applied to the final rendered image to add visual effects like bloom, depth of field, color grading, and more.

