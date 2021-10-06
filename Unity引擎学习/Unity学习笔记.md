# Unity学习笔记

## 1：使角色移动、转向

创建一个脚本，用`Input.GetAxisRaw`获取输入，然后判断若向素材相反的方向走，则用`transform.localScale=new Vector3(-1,1,1)`使之反转

用`transform.Translate(new Vector3(x,y,0))`来移动角色的位置。

## 2： 2D碰撞器

先把需要发生碰撞的对象放在对应的图层里。

在角色的脚本里定义两个私有变量：

```c#
    private BoxCollider2D _boxCollider;
    private RaycastHit2D _hit;
```

在Start函数里初始化BoxCollider2D。

```c#
_boxCollider = GetComponent<BoxCollider2D>();
```

判断是否发生碰撞，如果没有碰撞则允许移动

```c#

        //确认可以在该方向上移动，如果box返回一个null，则可以移动
        _hit = Physics2D.BoxCast(
            //当前的位置
            transform.position,
            //碰撞器的大小
            _boxCollider.size,
            //角度，因为一次只判断一个方向，所以是0
            0,
            //想要移动的方向
            new Vector2(0,_velocity.y),
            //想要移动的距离
            Mathf.Abs(_velocity.y*Time.deltaTime),
            //和哪些图层碰撞，这里是和"Blocking","Actor"图层里的所有对象碰撞
            LayerMask.GetMask("Blocking","Actor")
            );
        if (_hit.collider==null)
        {
            transform.Translate(0,_velocity.y * Time.deltaTime,0);
        }
//确认可以在该方向上移动，如果box返回一个null，则可以移动
        _hit = Physics2D.BoxCast(
            transform.position,
            _boxCollider.size,
            0,
            new Vector2(_velocity.x,0),
            Mathf.Abs(_velocity.x*Time.deltaTime),
            LayerMask.GetMask("Blocking","Actor")
            );
        if (_hit.collider==null)
        {
            transform.Translate(_velocity.x * Time.deltaTime,0,0);
        }

```

## 3： 让摄像机跟随玩家移动

给摄像机创建一个脚本：

```c#

public class CameraMotor : MonoBehaviour
{
   
    //lookAt 指向玩家现在的位置
    public Transform lookAt;

    //允许镜头的中心与玩家位置的x轴的最大距离
    public float boundX = 0.15f;

    //允许镜头的中心与玩家位置的Y轴的最大距离
    public float boundY = 0.05f;

    private void LateUpdate()
    {
        //定义镜头即将移动的向量
        Vector3 delta = Vector3.zero;
        //镜头的中心与玩家位置的x轴的距离
        //获取玩家的位置和摄像机中心位置的差距
        var deltaX = lookAt.position.x - transform.position.x;
        // 如果差距比较大，要移动摄像头的位置
        if (deltaX > boundX || deltaX < -boundX)
        {
            // 判断移动的方向：如果玩家的位置在摄像头的右边，则摄像头向右边移动其差值
            //     使得摄像头和玩家距离重新达到边界值，这样就可以每一帧都在更新摄像头的位置，不会出现卡顿
            if (lookAt.position.x > transform.position.x)
            {
                delta.x = deltaX - boundX;
            }
            else
                // 如果玩家的位置在摄像头的左边，则摄像头向左边移动其差值
                // （此时deltaX是一个比-0.15f还要小的值，加上0.15f之后还是负值）
            {
                delta.x = deltaX + boundX;
            }
        }

        //同上，
        var deltaY = lookAt.position.y - transform.position.y;
        //如果y轴坐标差值大于允许的边界
        if (deltaY > boundY || deltaY < -boundY)
        {
            //判断玩家位置在摄像头的上方还是下方
            if (lookAt.position.y > transform.position.y)
                //如果玩家的位置在摄像头中心的上方，则摄像头向上移动
            {
                delta.y = deltaY - boundY;
            }
            else
                //如果玩家的位置在摄像头中心的下方，则摄像头向下移动
                //此时的deltaY是一个比-0.05f还小的负值，加完0.05f还是负值，则是向下移动
            {
                delta.y = deltaY + boundY;
            }
        }
        transform.Translate(delta.x, delta.y, 0);
    }
}
```

## 4:TileMap的使用

先在地图集里切好16x16像素的素材，然后在层级里创建一个矩形瓦片地图。

打开窗口-2D-平铺调色板，创建一个新的调色板，放在合适的位置，然后把刚才切出来的素材丢进去，如果要编辑地图的布局，点调色板名称右边的编辑，然后按S键选择一块瓦片，再按M键，进入移动模式，然后按住不送移动到合适的位置，取消选择编辑即可保存。

不同的TileMap之间，使用 **排序图层** 顺序来设计前后顺序。

如果要用TileMap创建碰撞区域，创建一个Tile Map之后，用任意素材画好，然后取消勾选Tilemap Renderer，添加一个Tilemap Collider 2D，记得把该TileMap加入Blocking图层即可。

