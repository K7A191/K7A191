using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class playermovement : MonoBehaviour
{

    public float speed;
    public float jump;
    private float moveinput;

    private Rigidbody2D rb;

    private bool facingright = true;

    private bool isgrouded;
    public Transform groundcheck;
    public float checkradius;
    public LayerMask whatisground;

    private int extraJump;
    public int extraJumpValue;



    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        extraJump = extraJumpValue;
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        isgrouded = Physics2D.OverlapCircle(groundcheck.position, checkradius, whatisground);
        
        
        
        moveinput = Input.GetAxisRaw("Horizontal");
        rb.velocity = new Vector2(moveinput * speed, rb.velocity.y);

        if(facingright == false && moveinput > 0)
        {
            flip();
        }
        else if (facingright == true && moveinput < 0)
        {
            flip();
        }
    }

    private void Update()
    {
        if(isgrouded == true)
        {
            extraJump = extraJumpValue;
        }

        if (Input.GetKeyDown(KeyCode.W) && extraJump > 0)
        {
            rb.velocity = Vector2.up * jump;
            extraJump--;
        }
        else if (Input.GetKeyDown(KeyCode.W) && extraJump == 0 && isgrouded == true)
        {
            rb.velocity = Vector2.up * jump;
        }
    }

    void flip()
    {
        facingright = !facingright;
        Vector3 scaler = transform.localScale;
        scaler.x *= -1;
        transform.localScale = scaler;
    }
}

