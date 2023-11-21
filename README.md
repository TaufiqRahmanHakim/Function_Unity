# Function_Unity

## Resume and Pause
```csharp
public class ExampleScript : MonoBehaviour {
    void Pause() {
        Time.timeScale = 0;
    }
    
    void Resume() {
        Time.timeScale = 1;
    }
}
```

## Creating and Destroying GameObjects
```csharp
public GameObject enemy;

void Start() {
    for (int i = 0; i < 5; i++) {
        Instantiate(enemy);//GameObject can be created using the Instantiate function which makes a new copy of an existing object:
    }
}
```
```csharp
void OnCollisionEnter(Collision otherObj) {
    if (otherObj.gameObject.tag == "Missile") {
        Destroy(gameObject,.5f);//will destroy an object after the frame update has finished or optionally after a short time delay:
    }
}
```
```csharp
Destroy(this); //can destroy individual components without affecting the GameObject itself
```
