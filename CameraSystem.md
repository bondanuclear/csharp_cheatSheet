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
