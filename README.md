# gsoc
Compose Multiplatform component gallery generator
/ComposeGallery
 ├── /androidApp       # Android-specific code
 ├── /desktopApp       # Desktop-specific code
 ├── /webApp           # Web-specific code
 ├── /shared           # Shared UI Components
 │   ├── /src/commonMain/kotlin/com/gallery
 │   │    ├── GalleryScreen.kt  # Main gallery UI
 │   │    ├── Components.kt     # Example UI Components
 │   ├── build.gradle.kts
 ├── settings.gradle.kts
 ├── build.gradle.kts
 ├── README.md
package com.gallery

import androidx.compose.foundation.layout.*
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

@Composable
fun GalleryScreen() {
    Scaffold(
        topBar = {
            TopAppBar(title = { Text("Compose Multiplatform Gallery") })
        }
    ) { padding ->
        Column(modifier = Modifier.padding(padding).padding(16.dp)) {
            Text("Component Previews", style = MaterialTheme.typography.h5)

            Spacer(modifier = Modifier.height(16.dp))

            // Example UI Components
            ButtonExample()
            Spacer(modifier = Modifier.height(16.dp))
            TextFieldExample()
        }
    }
}
package com.gallery

import androidx.compose.foundation.layout.*
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

@Composable
fun ButtonExample() {
    var clicked by remember { mutableStateOf(false) }
    
    Button(onClick = { clicked = !clicked }) {
        Text(if (clicked) "Clicked!" else "Click Me")
    }
}

@Composable
fun TextFieldExample() {
    var text by remember { mutableStateOf("") }

    OutlinedTextField(
        value = text,
        onValueChange = { text = it },
        label = { Text("Enter text") },
        modifier = Modifier.fillMaxWidth().padding(8.dp)
    )
}
package com.gallery

import androidx.compose.ui.window.singleWindowApplication

fun main() = singleWindowApplication {
    GalleryScreen()
}
package com.gallery

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            GalleryScreen()
        }
    }
}
plugins {
    kotlin("multiplatform")
    id("org.jetbrains.compose") version "1.5.0"
}

kotlin {
    jvm() // Desktop
    android()
    js(IR) {
        browser()
    }

    sourceSets {
        val commonMain by getting {
            dependencies {
                implementation(compose.runtime)
                implementation(compose.foundation)
                implementation(compose.material)
            }
        }
    }
}

android {
    compileSdk = 34
    defaultConfig {
        minSdk = 21
    }
}
