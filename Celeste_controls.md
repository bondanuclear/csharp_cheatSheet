# Основні механіки для гри, схожої на Celeste.
## Клас PlayerInput - збирає всю інформацію від гравця. Клас PlayerManager - збирає в собі механіки усіх класів. Необхідний для наслідування SRP принципу.
### Клас PlayerMovement - клас з основними механіками
```csharp
/// Smooth Dash
// for dash to work properly we have to create simple state machine using enums with states: walking, dashing, idle etc
// or use state pattern (but let's try to not overkill with redundant complexity, only big projects)
private void HandleSmoothDash()
    {   
        if(canDash)
        {
            rigidbody2D.velocity = movDir.normalized * smoothDashPower;
            hasUsedDash = true; // used dash to prevent infinite dashes
            timeSinceLastDash = 0; // used for cooldown
        }
    }
  when state == State.Dashing
  case State.Dashing:
                float downMultiplyer = 3f;
                smoothDashPower -= smoothDashPower * downMultiplyer * Time.deltaTime;
                float minimumDashSpeed = 6f;
                if (smoothDashPower <= minimumDashSpeed)
                {
                    state = State.Walking;
                }
                break;
  // so we are slowing down towards the end of dash                
                
/// Instant Dash
private void HandleDash()
    {
        //Debug.Log($"Should dash in {movDir} direction");
        // teleporting
        rigidbody2D.MovePosition((Vector2)transform.position + movDir.normalized * dashPower);
    }
/// To rotate the sprite just set the x in the transform.localScale to -1
private void CheckSpriteRotation()
    {
        if(movDir.x > 0) transform.localScale = Vector3.one;
        else if (movDir.x < 0) transform.localScale = new Vector3(-1, transform.localScale.y, transform.localScale.z); 
    }
// falling logic && stop jumping when unpress jump button
private void FallingGravity()
    {
        
        if (rigidbody2D.velocity.y < jumpFallOff || !Input.GetKey(KeyCode.C) && rigidbody2D.velocity.y > 0)
        {
            
            rigidbody2D.velocity += Vector2.down * 9.8f * Time.fixedDeltaTime * fallMultiplyer;
        }
    }
// coyote jump and ordinary jump
private void CheckCoyoteJump()
    {
        if(isGrounded)
        {
            coyoteCounter = timeToCoyoteJump;
        } else if(!isGrounded)
        {
            coyoteCounter -= Time.deltaTime;
        }
        if(!Input.GetKey(KeyCode.C) && rigidbody2D.velocity.y > 0)
        {
            coyoteCounter = 0;
        }
    }
  private void HandleJumping()
    {
        if (coyoteCounter > 0)
        {            
            rigidbody2D.velocity = new Vector2(rigidbody2D.velocity.x, jumpForce);
            //isGrounded = false;
        }               
    }    
```

