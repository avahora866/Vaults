---
tags:
  - Vue
---
Setting up a Vue.js project can be done easily using the Vue CLI (`vue create project-name`) or Vite. Vite is the modern build tool recommended for Vue 3 projects because of its fast development server and optimized build process. Here’s how you can set up a Vue project using both methods:

# Using Vue CLI (`vue create project-name`)

The Vue CLI is a command-line tool that helps set up and manage Vue.js projects with a range of options. It automates much of the configuration and setup process.

## Steps:

1. **Install Vue CLI** (if you haven't already): First, you need to install the Vue CLI globally on your machine. Open your terminal and run:

```
npm install -g @vue/cli
```

2.   
   **Create a New Vue Project**: Run the following command to create a new Vue project:

```
vue create project-name
```

3.   
   **Select Project Configuration**: After running the `vue create` command, Vue CLI will prompt you to select the configuration you want. You can choose between:

   * **Default (Vue 3, Babel, ESLint)**: This is a simple, ready-to-go setup with Vue 3\.

   * **Manually select features**: If you want more control, you can manually select features like Vue Router, Vuex, TypeScript, CSS Pre-processors, Linting, etc.

4. **Navigate to Your Project**: After the setup completes, navigate to the newly created directory:

```
cd project-name
```

5.   
   **Run the Development Server**: Now, start the development server by running:

```
npm run serve
```

6.   
   This will launch a local server at `http://localhost:8080/`, and you can start developing your Vue app.

# Using Vite (Recommended for Vue 3\)

Vite is a build tool that is faster than Vue CLI, especially in terms of development server startup. It also provides optimized production builds with automatic code splitting.

## Steps:

1. **Install Vite**: You don’t need to install Vite globally. Instead, you can use `npm create` or `yarn create` to set up the project with Vite. Here’s the command using `npm`:

```
npm create vite@latest project-name --template vue
```

   *   
     `vite@latest`: Ensures you're using the latest version of Vite.

   * `project-name`: The name of your project folder.

   * `--template vue`: Tells Vite to set up a Vue project (this uses Vue 3 by default).

2. **Navigate to Your Project**: Once the project setup is complete, go into the project folder:

```
cd project-name
```

3.   
   **Install Dependencies**: After navigating to the project directory, install the required dependencies:

```
npm install
```

4.   
   **Run the Development Server**: You can now start the development server using:

```
npm run dev
```

5.   
   By default, Vite will serve the app on `http://localhost:5173/`.

6. **Start Developing**: Open the project in your favorite code editor and start modifying files. Vite provides fast hot module replacement (HMR), so your changes will reflect instantly in the browser without needing to refresh the page.

---

# Comparing Vue CLI and Vite:

| Feature | Vue CLI | Vite |
| ----- | ----- | ----- |
| **Build Speed** | Slower, especially on larger projects | Very fast, instant server start and HMR |
| **Default Setup** | Comprehensive with many features | Minimal, with Vue 3 template by default |
| **Configuration** | More out-of-the-box configuration options | Less configuration, modern defaults |
| **Development Speed** | Slower build times, more configuration overhead | Fast, modern development experience |

# Conclusion:

* **Use Vue CLI** if you need a more extensive setup or are working on an older Vue 2 project (though Vue 3 is supported as well).

* **Use Vite** if you want the modern, faster development experience with Vue It’s the recommended option for new projects and provides a better overall performance.

For most modern Vue.js projects, **Vite** is the recommended choice due to its speed and simplicity.

