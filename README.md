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

## Camera Rotation
```csharp
[Tooltip("How far in degrees can you move the camera up")]
public float TopClamp = 70.0f;

[Tooltip("How far in degrees can you move the camera down")]
public float BottomClamp = -30.0f;
private StarterAssetsInputs _input;
private PlayerInput _playerInput;
public GameObject CinemachineCameraTarget;

private const float _threshold = 0.01f;
public float CameraAngleOverride = 0.0f;

private float _cinemachineTargetYaw;
private float _cinemachineTargetPitch;

public bool LockCameraPosition = false;
private bool IsCurrentDeviceMouse
        {
            get
            {
#if ENABLE_INPUT_SYSTEM
                return _playerInput.currentControlScheme == "KeyboardMouse";
#else
				return false;
#endif
            }
        }
[NonSerialized] private InputUser m_InputUser;
public string currentControlScheme
        {
            get
            {
                if (!m_InputUser.valid)
                    return null;

                var scheme = m_InputUser.controlScheme;
                return scheme?.name;
            }
        }


private void CameraRotation()
        {
            // if there is an input and camera position is not fixed
            if (_input.look.sqrMagnitude >= _threshold && !LockCameraPosition)
            {
                //Don't multiply mouse input by Time.deltaTime;
                float deltaTimeMultiplier = IsCurrentDeviceMouse ? 1.0f : Time.deltaTime;

                _cinemachineTargetYaw += _input.look.x * deltaTimeMultiplier;
                _cinemachineTargetPitch += _input.look.y * deltaTimeMultiplier;
            }

            // clamp our rotations so our values are limited 360 degrees
            _cinemachineTargetYaw = ClampAngle(_cinemachineTargetYaw, float.MinValue, float.MaxValue);
            _cinemachineTargetPitch = ClampAngle(_cinemachineTargetPitch, BottomClamp, TopClamp);

            // Cinemachine will follow this target
            CinemachineCameraTarget.transform.rotation = Quaternion.Euler(_cinemachineTargetPitch + CameraAngleOverride,
                _cinemachineTargetYaw, 0.0f);
        }
```
