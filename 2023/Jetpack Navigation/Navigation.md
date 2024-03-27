slidenumbers: true
autoscale: true

# Android Jetpack Navigation
### Artur Badretdinov

---

# What is Android Navigation?
- Changing what users see on the screen
- Screens
- Transitions
- Animations
- Passing params forward
- Returning results

---

# Navigation Component Overview
- Navigation Graph
- NavHost
- NavController

---

# Navigation Graph
- Resource collecting all navigation data
- Destinations and paths in your app

---

# NavHost
- A unique composable in layout
- Shows destinations from Navigation Graph

---

# NavController
- Central API of Navigation Component
- Manages composable screens and their state

---

# Jetpack Compose Integration
- Support for navigating between composables
- Leverages Navigation component's features

---

# Frament Integration
- Support for navigating between Fragments
- The base concepts are the same

---

# Dependencies

```
dependencies {
  def nav_version = "2.7.6"

  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-compose:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

  // Feature module Support
  implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

  // Testing Navigation
  androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"

}
```

---

# NavController in Jetpack Compose
- Central API for Navigation
- Use `rememberNavController()` method

---

# NavHost Creation
- Links NavController with a navigation graph
- Route associated with each composable destination

---

# Code Example: NavController

```
val navController = rememberNavController()
```

---

# Code Example: NavHost

```
NavHost(navController = navController, startDestination = "profile") {
    composable("profile") { Profile(/*...*/) }
    composable("friendslist") { FriendsList(/*...*/) }
    /*...*/
}
```

---

# Defining Navigation in App
1. Define screen names and routes (AppNavigation.kt)
2. Define NavHost with screens (AppNavHost.kt)
3. Call NavHost in MainActivity.kt

---

# Step 1: Define Screen Names and Routes

```
enum class Screen {
    HOME,    
    LOGIN,
}
sealed class NavigationItem(val route: String) {
    object Home : NavigationItem(Screen.HOME.name)
    object Login : NavigationItem(Screen.LOGIN.name)
}
```

---

# Step 2: Define NavHost with Screens

```
@Composable
fun AppNavHost(
    modifier: Modifier = Modifier,
    navController: NavHostController,
    startDestination: String = NavigationItem.Splash.route,
    ... // other parameters
) {
    NavHost(
        modifier = modifier,
        navController = navController,
        startDestination = startDestination
    ) {
        composable(NavigationItem.Splash.route) {
            SplashScreen(navController)
        }
        composable(NavigationItem.Login.route) {
            LoginScreen(navController)
        }
}
```

---

# Step 3: Call NavHost in MainActivity

```
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            AutoPartsAppTheme {
               Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    AppNavHost(navController = rememberNavController())
                }
            }
        }
    }
}
```

---

# Navigation Argument Types
1. Without Arguments
2. Simple Arguments (e.g., Int, String)
3. Complex/User-Defined Data Type
4. Optional Arguments
5. Navigate Back with Result

---

# Navigation Without Argument

```
NavHost(navController = navController, startDestination = "profile") {
    composable("profile") { Profile(/*...*/) }
    composable("friendslist") { FriendsList(/*...*/) }
    /*...*/
}
```

---

# Navigation With Simple Arguments

```
NavHost(startDestination = "profile/{userId}") {
    ...
    composable("profile/{userId}") {...}
}
```

---

# Navigation With Simple Arguments

```
composable("profile/{userId}") { backStackEntry ->
   val userId = backStackEntry.arguments?.getString("userId")
   // here you have to fetch user data 
   Profile(
      navController, 
      //pass fetch user data ie. UserInfo
   )
}
```

---

# Navigation With Complex Arguments

- Pass id
- Extract the item from repo
- Display on the screen

---

# Optional Arguments in Navigation

- Must be included using query parameters
- Must have defaultValue or nullable = true

---

# Optional Arguments in Navigation

```
composable(
    "profile?userId={userId}/{isMember}",
    arguments = listOf(
         navArgument("userId") {
            type = NavType.StringType
            defaultValue = "user1234"
           // OR 
            type = NavType.StringType
            nullable = true
         },
         navArgument("isNewTask") {
            type = NavType.BoolType
         }
     )
) { backStackEntry ->
    val userId = backStackEntry.arguments?.getString("userId")
    val isMember = backStackEntry.arguments?.getBoolean("isMember")?:false
    Profile(navController, userId, isMember)
}
```

---

# Navigating Back with Result. Put

```
@Composable
fun SecondScreen(navController: NavController) {
    var text by remember {
        mutableStateOf("")
    }
     Column( /* modifiers */ ) {
        TextField(
            value = text, onValueChange = { text = it },
            placeholder = { Text("Enter text") }
        )
        Spacer(Modifier.height(8.dp))
        Button(onClick = {
            navController.previousBackStackEntry?.savedStateHandle?.set("msg", text)
            navController.popBackStack()
        }) {
            Text(text = "Submit")
        }
    }
}
```

---

# Navigating Back with Result. Read

```
@Composable
fun FirstScreen(navController: NavController) {
    // Retrieve data from next screen
    val msg = 
        navController.currentBackStackEntry?.savedStateHandle?.get<String>("msg")
    Column( /* modifiers */ ) {
        Button(onClick = { navController.navigate("secondscreen") }) {
            Text("Go to next screen")
        }
        Spacer(modifier = Modifier.height(8.dp))
        msg?.let {
            Text(it)
        }
    }
}
```

---

# Deep Links in Navigation

```
val uri = "https://jobagent.pro"
composable(
    "profile?id={id}",
    deepLinks = listOf(navDeepLink { uriPattern = "$uri/{id}" })
) { backStackEntry ->
    Profile(navController, backStackEntry.arguments?.getString("id"))
}
```

---

# External Deep Links registration

```
<activity …>
  <intent-filter>
    ...
    <data android:scheme="https" android:host="www.example.com" />
  </intent-filter>
</activity>
```

---

# Nested Navigation in Jetpack Compose
- Group destinations into a nested graph
- Modularize specific flows in the app UI
- Requires a designated start destination

---

# Creating a Nested Graph
- Use the `navigation` extension function in `NavHost`
- Example: Self-contained login flow
- Automatically navigate to the graph's start destination

---

# Code Example: Nested Graph

```
NavHost(navController, startDestination = "home") {
    ...
    // Navigating to the graph via its route ('login') automatically
    // navigates to the graph's start destination - 'username'
    // therefore encapsulating the graph's internal routing logic
    navigation(startDestination = "username", route = "login") {
        composable("username") { ... }
        composable("password") { ... }
        composable("registration") { ... }
    }
    ...
}
```

---

# Splitting the Navigation Graph
- Recommended for larger graphs
- Allows multiple modules to contribute their navigation graphs
- Use extension methods on `NavGraphBuilder`

---

# Code Example: Splitting Navigation Graph

```
fun NavGraphBuilder.loginGraph(navController: NavController) {
    navigation(startDestination = "username", route = "login") {
        composable("username") { ... }
        composable("password") { ... }
        composable("registration") { ... }
    }
}

NavHost(navController, startDestination = "home") {
    ...
    loginGraph(navController)
    ...
}
```

---

# Questions?
