||Prac 2||
Rotation.cs
-----------------------------------------------------------------------
using UnityEngine;
public class Rotation : MonoBehaviour
{
    public float speed = 200f;
    private Rigidbody2D rb2d;

    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        float rotateInput = Input.GetAxis("Horizontal");
        rb2d.angularVelocity = -rotateInput * speed;       
    }
}
-----------------------------------------------------------------
Movement+Scaling.cs

using UnityEngine;
public class NewBehaviourScript : MonoBehaviour
{
    public float speed = 5f;
    private Rigidbody2D rb2d;

    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        float moveX = Input.GetAxis("Horizontal");
        float moveY = Input.GetAxis("Vertical");

        Vector2 movement = new Vector2(moveX, moveY);
        rb2d.linearVelocity = movement * speed;

        if (Input.GetKey(KeyCode.Q))
        {
            transform.localScale += Vector3.one * Time.deltaTime;
        }
        if (Input.GetKey(KeyCode.E))
        {
            transform.localScale -= Vector3.one * Time.deltaTime;

            if (transform.localScale.x < 0.1f)
                transform.localScale = new Vector3(0.1f, 0.1f, transform.localScale.z);
        }
    }
}
-------------------------------------------------------------------
||Prac 3||

Color+Movement+Jump.cs
using UnityEngine;

public class jump : MonoBehaviour
{
    public float jumpforce = 5f;
    public float speed = 5f;
    public Rigidbody2D rb2d;
    public SpriteRenderer sr;
    public Color leftcol = Color.blue;
    public Color rightcol = Color.green;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        rb2d = GetComponent<Rigidbody2D>();
        sr = GetComponent<SpriteRenderer>();
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            rb2d.linearVelocity = new Vector2(rb2d.linearVelocity.x, jumpforce);
        }
        float movex=Input.GetAxis("Horizontal");
        rb2d.linearVelocity= new Vector2(movex * speed, rb2d.linearVelocity.y);
        if (movex > 0)
        {
            sr.color = rightcol;
        }
        else if (movex < 0)
        {
            sr.color = leftcol;
        }
    }
}
---------------------------------------------------------------
||Prac 4||

ButtonAction.cs
using UnityEngine;

public class ButtonAction : MonoBehaviour
{
    public void SayHello()
    {
        Debug.Log("Hello");
    }
}

---------------------------------------------------------------
ToggleCode.cs

using UnityEngine;

public class ToggleCode : MonoBehaviour
{
    public void UserValueChanged(bool value)
    {
        Debug.Log("Toggle Value Changed: " + value);
    }
}

---------------------------------------------------------------
SliderConsole.cs

using UnityEngine;

public class SliderConsole : MonoBehaviour
{
    public void SliderValueChanged(float value)
    {
        Debug.Log("Slider Value: " + value);
    }
}
---------------------------------------------------------------
InputCode.cs

using UnityEngine;
using TMPro;

public class InputCode : MonoBehaviour
{
    public void ReadInput(string text)
    {
        Debug.Log("User Typed: " + text);
    }
}
---------------------------------------------------------------
DropdownCode.cs

using UnityEngine;
using TMPro;

public class Dropdowns : MonoBehaviour
{
    public void DropdownValueChanged(int index)
    {
        Debug.Log("Selected Option Index: " + index);
    }
}
---------------------------------------------------------------


||Prac 5||

Moving.cs

using UnityEngine;


public class jump : MonoBehaviour
{
    public Rigidbody2D rb;
    public SpriteRenderer sr;
    public float jumpforce = 5f;
    public float speed = 5f;


    public Color leftcol = Color.pink;
    public Color rightcol = Color.purple;
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
        sr = GetComponent<SpriteRenderer>();
    }


    // Update is called once per frame
    void Update()
    {
        float moveX = Input.GetAxis("Horizontal");
        rb.linearVelocity = new Vector2(moveX * speed, rb.linearVelocity.y);
        if (moveX < 0)
        {
            sr.color=leftcol;
        }
        else if (moveX > 0)
        {
            sr.color = rightcol;
        }
        if (Input.GetKeyDown(KeyCode.Space))
        {
            rb.linearVelocity = new Vector2(rb.linearVelocity.x, jumpforce);
        }
    }
}
---------------------------------------------------------------

Audio.cs


using UnityEngine;

[RequireComponent(typeof(AudioSource))]
public class audio : MonoBehaviour
{
    AudioSource a;
    void Awake()
    {
        a = GetComponent<AudioSource>();


    }
    void OnTriggerEnter2D(Collider2D o)
    {
        if (o.CompareTag("Player")) a.Play();
    }


    void OnTriggerExit2D(Collider2D o)
    {
        if (o.CompareTag("Player")) a.Stop();
    }
}
---------------------------------------------------------------

||Prac 6||

SceneLoader.cs

using UnityEngine;
using UnityEngine.SceneManagement;

public class SceneLoader : MonoBehaviour
{
    // Load a scene by name
    public void LoadScene(string sceneName)
    {
        SceneManager.LoadScene(sceneName);
    }

}

---------------------------------------------------------------

SceneOperations.cs

using UnityEngine;
using UnityEngine.SceneManagement; // Required for Scene Management

public class SceneOperations : MonoBehaviour
{
    public void LoadStandard()
    {
        SceneManager.LoadScene("SecondScene", LoadSceneMode.Single);
    }

    public void LoadAdditive()
    {
        if (!SceneManager.GetSceneByName("SecondScene").isLoaded)
        {
            SceneManager.LoadScene("SecondScene", LoadSceneMode.Additive);
        }
    }

    public void UnloadScene()
    {
        if (SceneManager.GetSceneByName("SecondScene").isLoaded)
        {
            SceneManager.UnloadSceneAsync("SecondScene");
        }
    }

    public void LoadAsync()
    {
        SceneManager.LoadSceneAsync("SecondScene", LoadSceneMode.Single);
    }
}


---------------------------------------------------------------

||Prac 7||

Range+Angle.cs

using UnityEngine;

public class rangechecker : MonoBehaviour
{
    [SerializeField] Transform target;
    [SerializeField] public float range = 15f;
    public float speed = 5f;
    private bool targetinrange = false;
    // Update is called once per frame
    void Update()
    {
        float distance = (target.position - transform.position).magnitude;
        var direction = (target.position - transform.position).normalized;
        var dotp = Vector3.Dot(transform.forward, direction);
        var angle = Mathf.Acos(dotp);
        Debug.LogFormat(
            "The between my forward direction and {0} is {1:F1}",
            target.name,
            angle * Mathf.Rad2Deg
        );


        if (distance <= range && targetinrange == false)
        {
            Debug.LogFormat("Entered yay", target.name);
            targetinrange = true;
        }
        else if (distance > range && targetinrange == true)
        {
            Debug.LogFormat("Nooo LEft", target.name);
            targetinrange = false;
        }
        float moveX = 0f;
        float moveY = 0f;
        float moveZ = 0f;


        if (Input.GetKey(KeyCode.A))
        {
            moveX = -1f;
        }
        if (Input.GetKey(KeyCode.D))
        {
            moveX = +1f;
        }
        if (Input.GetKey(KeyCode.W))
        {
            moveY = +1f;
        }
        if (Input.GetKey(KeyCode.S))
        {
            moveY = -1f;
        }
        if (Input.GetKey(KeyCode.Q))
        {
            moveZ = +1f;
        }
        if (Input.GetKey(KeyCode.E))
        {
            moveZ = -1f;
        }
        Vector3 move = new Vector3(moveX, moveY, moveZ);
        transform.Translate(move * speed * Time.deltaTime, Space.World);
    }
}

---------------------------------------------------------------

||Prac 8||

Player Move.cs

using UnityEngine;


public class playermove : MonoBehaviour
{
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    public float speed = 7f;
    Rigidbody rb;
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }


    // Update is called once per frame
    void Update()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        Vector3 move = new Vector3(h, 0, v) * speed;
        rb.linearVelocity = new Vector3(move.x, 0, move.z);
    }
}
---------------------------------------------------------------
Ai moves.cs

using UnityEngine;
using UnityEngine.AI;


public class Aimoves : MonoBehaviour
{
    public Transform target;
    public float speed = 5f;
    private NavMeshAgent agent;
    // Start is called once before the first execution of Update after the MonoBehaviour is created
    void Start()
    {
        agent = GetComponent<NavMeshAgent>();
        agent.speed=speed;
    }


    // Update is called once per frame
    void Update()
    {
        if (target != null) {
            agent.SetDestination(target.position);
        }
    }
}

---------------------------------------------------------------

Goal code.cs

using UnityEngine;

public class GOAL : MonoBehaviour
{
    public void Trigger(Collider other)
    {
        if (other.CompareTag("Player"))
        {
            Debug.Log("You Win");
            Time.timeScale = 0;
        }
        else if (other.CompareTag("Enemy"))
        {
            Debug.Log("Ai wins");
            Time.timeScale = 0;
        }
    }
}

---------------------------------------------------------------

Game Manager.cs
using UnityEngine;


public class GameManager : MonoBehaviour
{
    public GameObject player;
    public GameObject ai;
    public GameObject goal;
    // Update is called once per frame
    void Update()
    {
        float playerdist = Vector3.Distance(player.transform.position, goal.transform.position);
        float aidist = Vector3.Distance(ai.transform.position, goal.transform.position);
        Debug.Log(aidist);
        Debug.Log(playerdist);
        if (playerdist < 3.2)
        {
            Debug.Log("Player Wins");
            Quit();
        }
        else if (aidist < 3.2)
        {
            Debug.Log("Ai Wins");
            Quit();
        }
    }
    void Quit()
    {
        Application.Quit();
        UnityEditor.EditorApplication.isPlaying=false;
    }
}

---------------------------------------------------------------
||Prac 9||

using UnityEngine;

public class PlayerAnim : MonoBehaviour
{
    public Animator animator;
    void Start()
    {
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        animator.SetBool("run", Input.GetKeyDown(KeyCode.W));
        animator.SetBool("walk", Input.GetKeyDown(KeyCode.E));
    }
}


