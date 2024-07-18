# Unity camera system contols using cinemachine
## variables + awake/update methods
``` csharp
    [SerializeField] float cameraMoveSpeed = 10f;
    [SerializeField] float rotationSpeed = 10f;
    [SerializeField] float edgeOffset = 10f;
    [SerializeField] bool canEdgeScroll = false;
    bool isDraggingActive = false;
    [SerializeField] Vector2 startPos = new Vector2();
    [SerializeField] float dragSpeed = 30f;
    [SerializeField] float mouseRotationSpeed = 40f;
    [SerializeField] CinemachineVirtualCamera myCamera;
    [Header("Camera zoom settings")]
    [SerializeField] Vector3 followOffset = new Vector3();
    [SerializeField] float maxFollowOffset = 80;
    [SerializeField] float minFollowOffset = 15;
    [SerializeField] float zoomingSpeed = 10f;
    ///
    [SerializeField] float maxYValue;
    [SerializeField] float minYValue = 20f;
    float currFieldOfView;
    private void Awake() {
        currFieldOfView = myCamera.m_Lens.FieldOfView;
        followOffset = myCamera.GetCinemachineComponent<CinemachineTransposer>().
        m_FollowOffset;
        maxYValue = followOffset.y;
    }
    void Update()
    {
        if (Input.GetMouseButtonDown(1))
        {
            isDraggingActive = true;
            startPos = new Vector2(Input.mousePosition.x, Input.mousePosition.y);
        }
        if (Input.GetMouseButtonUp(1))
        {
            isDraggingActive = false;
        }
        // movement
        HandleMovement();
        if(canEdgeScroll) HandleEdgeScrolling();
        if (isDraggingActive) 
        {
            HandleDragging();
            //HandleRotationMouse();
        }
        // rotation
        HandleRotation();
        //HandleZooming();
        //HandleZooming_FieldOfView();
        HandleZooming_LowerY();
    }

```
## Camera movement
### MoveDir is normalized, so we can't go faster when go diagonally
``` csharp
private void HandleMovement()
    {
        Vector3 inputDirection = new Vector3();

        if (Input.GetKey(KeyCode.W)) inputDirection.z = 1;
        if (Input.GetKey(KeyCode.A)) inputDirection.x = -1;
        if (Input.GetKey(KeyCode.S)) inputDirection.z = -1;
        if (Input.GetKey(KeyCode.D)) inputDirection.x = 1;
       
        Vector3 moveDir = (transform.forward * inputDirection.z + transform.right * inputDirection.x).normalized;
        transform.position += moveDir * cameraMoveSpeed * Time.deltaTime;
    }
```
## Camera rotation
### first - rotation with mouse - uncompleted, because it's more difficult to implement x-rotation, but still works. Second - input-based rotation
``` csharp
private void HandleRotationMouse()
    {
        Vector2 direction = -((Vector2)Input.mousePosition - startPos);
        Debug.Log("DIRECTION = " + direction);
        transform.eulerAngles += new Vector3(0,direction.x * Time.deltaTime * dragSpeed,0) ;
        startPos = Input.mousePosition; // so we do not rotate endlessly
    }
    private void HandleRotation()
    {
        float rotateDirection = 0;
        if (Input.GetKey(KeyCode.Q)) rotateDirection = 1;
        if (Input.GetKey(KeyCode.E)) rotateDirection = -1; // direction of rotation
        transform.eulerAngles += new Vector3(0, rotateDirection * rotationSpeed * Time.deltaTime, 0);
    }
```
## Camera dragging - similar to dota 2 mini-map dragging
### watch update to see booleans for this method
``` csharp
private void HandleDragging()
    {
        Vector3 inputDirection = new Vector3();
        Vector2 direction = (Vector2)Input.mousePosition - startPos;
        inputDirection.x = direction.x * dragSpeed;
        inputDirection.z = direction.y * dragSpeed;
        startPos = Input.mousePosition; // so we do not move endlessly
        Vector3 moveDir = (transform.forward * inputDirection.z + transform.right * inputDirection.x);
        transform.position += moveDir * cameraMoveSpeed * Time.deltaTime;
    }
```
## Camera Edge Scrolling - dota 2 mechanic for moving camera when cursor touches edges of screen
### notable properties - Screen.height and Screen.width
``` csharp
private void HandleEdgeScrolling()
    {
        Vector3 inputDirection = new Vector3();
        if ((Input.mousePosition.y + edgeOffset) >= Screen.height) inputDirection.z = 1;
        if ((Input.mousePosition.x - edgeOffset) <= 0) inputDirection.x = -1;
        if ((Input.mousePosition.y - edgeOffset) <= 0) inputDirection.z = -1;
        if ((Input.mousePosition.x + edgeOffset) >= Screen.width) inputDirection.x = 1;
        Vector3 moveDir = (transform.forward * inputDirection.z + transform.right * inputDirection.x).normalized;
        transform.position += moveDir * cameraMoveSpeed * Time.deltaTime;
    }
```
## Zooming camera
### To zoom camera, we have to get to the CinemachineTransposer component inside cinemachine camera
### First zoom - changing field of view value. Second - changing cinemachine follow offset. Third - descending by changing follow offset y axis
``` csharp 
private void HandleZooming_FieldOfView()
    {
        if (Input.mouseScrollDelta.y > 0)
        {
            currFieldOfView += 5f;
        }
        else if (Input.mouseScrollDelta.y < 0)
        {
            currFieldOfView -= 5f;
        }
        currFieldOfView = Mathf.Clamp(currFieldOfView, 10, 60);
        myCamera.m_Lens.FieldOfView = Mathf.Lerp(
            myCamera.m_Lens.FieldOfView,
            currFieldOfView,
            Time.deltaTime * zoomingSpeed
        );
    }
     private void HandleZooming()
    {
        Vector3 zoomDir = followOffset.normalized;
        if(Input.mouseScrollDelta.y > 0)
        {
            followOffset += zoomDir;
        }
        else if (Input.mouseScrollDelta.y < 0)
        {
            followOffset -= zoomDir;
        }
        // constraints    
        if(followOffset.magnitude > maxFollowOffset)
        {
            followOffset = zoomDir * maxFollowOffset;
        }

        if (followOffset.magnitude < minFollowOffset)
        {
            followOffset = zoomDir * minFollowOffset;
        }
        
        myCamera.GetCinemachineComponent<CinemachineTransposer>().m_FollowOffset = Vector3.Lerp(
        myCamera.GetCinemachineComponent<CinemachineTransposer>().m_FollowOffset, 
        followOffset,
        Time.deltaTime * zoomingSpeed);
    }
    private void HandleZooming_LowerY()
    {
        if (Input.mouseScrollDelta.y > 0)
        {
            followOffset.y += 2f;
        }
        else if (Input.mouseScrollDelta.y < 0)
        {
           followOffset.y -= 2f;
        }
        followOffset.y = Mathf.Clamp(followOffset.y, minYValue, maxYValue);

        myCamera.GetCinemachineComponent<CinemachineTransposer>().m_FollowOffset = Vector3.Lerp(
        myCamera.GetCinemachineComponent<CinemachineTransposer>().m_FollowOffset,
        followOffset,
        Time.deltaTime * zoomingSpeed);
    }
```
### Rotating Camera around object and following it without using Unity Engine methods
``` csharp
    [SerializeField] private Transform _followTarget;
    [SerializeField] private float _angleRotationX = 0;
    [SerializeField] private float _offsetY;
    [SerializeField] private Vector3 _offsetCameraPosition;
    [SerializeField] private float _distance = -10;
    private void LateUpdate() {
        if(_followTarget == null) return;

        //Vector3 position = _followTarget.position + _offsetCameraPosition;
        Quaternion rotation = Quaternion.Euler(_angleRotationX,0,0);
        Vector3 position = rotation * new Vector3(0,0, _distance) + (_followTarget.position + _offsetCameraPosition);

        transform.position = position;  
        transform.rotation = rotation;
    }
```
