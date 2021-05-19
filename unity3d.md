## Hierarchy(层级视图)

   Directional Light 平行光

## Scene(场景)

 

## GameObject

​     GameObject类是所有对象的基类

​     static  Find(string name)

​     static FindWithTag(String name)

​      Transform.Find()





## MonoBehaviour(定义脚本从加载到销毁的整个过程)

Awake:最先被调用，脚本加载到当前场景中会自动调用，执行一次

OnEnable:对象处于可激活状态时会自动调用 

Start:用于完成初始化，执行一次

FixedUpdate:用于更新场景，每一帧执行

Update: 每一帧都会被调用，每一帧执行

LateUpdate:在下一次更新前被调用，每一帧执行

OnGUI:

OnDisable:将对象设置为不可用或不可激活

OnDestroy：对象销毁



enable 

gameObject

GetComponent

Instantiate 克隆（原对象，新对象的位置，新对象的角度）

多个脚本怎么执行？

先绑定后执行



Unity中的组件

功能 ：Transform Mesh Filter/Renderer,Rigidbody

顶级父对象Transform.root

Transform relativeTo()

world世界坐标，self自己

Characters 人物

getMouseButton(0/1/2)(左/右/中)键

getAxis("Mouse X/Y")

mousePosition

Time.deltaTime

Mesh Filter:形状

Mesh Renderer:渲染

欧拉角（万向节死锁）

天空盒材质：6sided(6面),procedural（参数）,cubemmap三种

使用方式，1.摄像机clear flags 为skybox,摄像机添加组件skybox,2.window-lighting-environment lighting-skybox

Atmosphere Thickne 大气层

camera near 最近点

camera