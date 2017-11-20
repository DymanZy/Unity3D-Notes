##Unity中如何实现第一人称视觉

主要分两点来实现：

- 摄像机跟随人物行走
- 摄像机可受鼠标滑动控制





###实现摄像机跟随人物行走

这里也有两种办法，一种是将摄像机作为人物的子物体，这样在人物行走时摄像机将作为子物体而跟随移动，但是这种方法不太好的就是人物掉头时摄像机画面也跟着转向180°，体验不好。

所以我们一般是通过在摄像头的脚本上保留一份角色的Transform的引用，在Update中通过获取Transform的Position和Rotation以更新摄像头的Position和Rotation，以此实现第一人称视觉。

先创建两个脚本，分别是PersonController.cs和CameraController.cs。

```C#
public class PersonController : MonoBehaviour {
  
  public CameraController cameraControl;
  
  private Animator animator;
  private Vector2 input;
  private float speed;
  
  void Start() {
    animator = GetComponent<Animator>();
    cameraControl.BindTargetObject(transform);
  }
  
  void Update(){
    MovePerson();
  }
  
  void MovePerson() {
    input.x = Input.GetAxis("Horizontal");
    input.y = Input.GetAxis("Vertical");
  	
    // 左右转向
    if (input.x > 0) {
      transform.rotation = Quaternion.AngleAxis(90, Vector3.up);
    } else if (input.x < 0) {
      transform.rotation = Quaternion.AngleAxis(-90, Vector3.up);
    }
    
    // 后转
    if (input.y < 0 ) {
      transform.rotation = Quaternion.AngleAxis(180, Vector3.up);
    }
    
    // 向前行走
    speed = Mathf.Abs(input.x) + Mathf.Abs(input.y);
    speed = Mathf.Clamp(speed, 0f, 1f);
    animator.SetFloat("InputVertical", speed, 0.1f, Time.deltaTime);
  }
  
}
```



CameraController.cs

```C#
public class CameraController : MonoBehaviour {
  
  private Transform targetObject;
  
  public void BindTargetObject (Transform transform) {
    if (transform == null) return;
    this.targetObject = transform;
  }
  
  void Update() {
      ChangeCameraStatus();
  }
  
  private void ChangeCameraStatus() {
    transform.localPosition = new Vector3(targetObject.position.x, targetObject.position.y, targetObject.position.z);
  }
}
```

然后将这PersonController.cs挂载到场景中的人物上，CameraController.cs挂载到场景中的摄像头上。









