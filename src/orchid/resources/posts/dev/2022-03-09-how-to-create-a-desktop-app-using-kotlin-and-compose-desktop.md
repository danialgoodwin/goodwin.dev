---
title: How to create a desktop app using Kotlin and Compose Desktop
tags:
- kotlin
- compose
---

Let's quickly and simply create a desktop app using Kotlin and Compose for Desktop...

1. In IntelliJ IDEA, install the Compose Multiplatform plugin (Settings > Plugins, then search for and install 'Compose Multiplatform IDE Support' by JetBrains). Restart IJ to load the plugin.
2. In IntelliJ IDEA, click 'New Project', select 'Kotlin' on the left, then select 'Compose Desktop Application', then click 'Next'
3. Leave the defaults alone, then click 'Finish'
4. After the project loads, run 'Main.kt' to ensure that you have everything working. It should look similar to the following picture:

   ![Kotlin Compose for Desktop - Starting GUI](/assets/media/kotlin-compose-for-desktop-starting-gui.png)

5. Let's add an app bar. Replace the code in Main.kt with the following:

        import androidx.compose.material.MaterialTheme
        import androidx.compose.desktop.ui.tooling.preview.Preview
        import androidx.compose.material.Button
        import androidx.compose.material.Icon
        import androidx.compose.material.IconButton
        import androidx.compose.material.Scaffold
        import androidx.compose.material.Text
        import androidx.compose.material.TopAppBar
        import androidx.compose.material.icons.Icons
        import androidx.compose.material.icons.filled.Add
        import androidx.compose.material.icons.filled.Favorite
        import androidx.compose.material.icons.filled.Menu
        import androidx.compose.runtime.Composable
        import androidx.compose.runtime.getValue
        import androidx.compose.runtime.mutableStateOf
        import androidx.compose.runtime.remember
        import androidx.compose.runtime.setValue
        import androidx.compose.ui.window.Window
        import androidx.compose.ui.window.application
        
        @Composable
        @Preview
        fun App() {
            var text by remember { mutableStateOf("Hello, World!") }
        
            MaterialTheme {
                Scaffold(
                    topBar = { MyTopAppBar() }
                ) {
                    Button(onClick = {
                        text = "Hello, Desktop!"
                    }) {
                        Text(text)
                    }
                }
            }
        }
        
        fun main() = application {
            Window(onCloseRequest = ::exitApplication) {
                App()
            }
        }
        
        @Composable
        fun MyTopAppBar() {
            TopAppBar(
                title = { Text("Simple TopAppBar") },
                navigationIcon = {
                    IconButton(onClick = { /* doSomething() */ }) {
                        Icon(Icons.Filled.Menu, contentDescription = null)
                    }
                },
                actions = {
                    // RowScope here, so these icons will be placed horizontally
                    IconButton(onClick = { /* doSomething() */ }) {
                        Icon(Icons.Filled.Favorite, contentDescription = "Localized description")
                    }
                    IconButton(onClick = { /* doSomething() */ }) {
                        Icon(Icons.Filled.Add, contentDescription = "Localized description")
                    }
                }
            )
        }

6. Run the app again, and you should see the following:

    ![Kotlin Compose for Desktop - Add TopAppBar](/assets/media/kotlin-compose-for-desktop--add-topappbar.png)

To learn more about the available GUIs and views, see https://developer.android.com/jetpack/compose/layouts

<hr>

Now, let's create the executable file and run the app on the desktop!

1. In the terminal, at the root of the project, run `./gradlew createDistributable`
2. Confirm that your runnable app is in 'build/compose/binaries/main/app/'
3. If you are on Windows, then run the EXE (or MSI) file. The other supported targets/platforms are macOS (.dmg, .pkg) and Linux (.deb, .rpm)

To learn more about the distributions and local executions, see https://github.com/JetBrains/compose-jb/blob/master/tutorials/Native_distributions_and_local_execution/README.md

Congrats for making your first Kotlin desktop app! Feel free to reach out for any questions, comments, or support. :)
