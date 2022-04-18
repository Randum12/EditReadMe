# EditReadMe
Pong Game
# Paddle Movement
    public bool isPlayer1;
    public float speed;
    public Rigidbody2D rb;
    public Vector3 startPosition;

    private float movement;

    void Start()
    {
        startPosition = transform.position;
    }

    void Update()
    {
        if(isPlayer1)
        {
            movement = Input.GetAxisRaw("Vertical");
        }
        else
        {
            movement = Input.GetAxisRaw("Vertical2");
        }
    
        rb.velocity = new Vector2(rb.velocity.x, movement * speed);
    }

    public void Reset()
    {
        rb.velocity = Vector2.zero;
        transform.position = startPosition;
    }
   
   # Ball Movement
      public float speed;
   public Rigidbody2D rb;
   public Vector3 startPosition;

   void Start()
    {
        Launch();
    }

    public void Reset()
    {
        rb.velocity = Vector2.zero;
        transform.position = startPosition;
        Launch();
    }

    private void Launch()
    {
        float x = Random.Range(0, 2) == 0 ? -1:1;
        float y = Random.Range(0, 2) == 0 ? -1:1;
        rb.velocity = new Vector2(speed * x, speed * y);
    }
    
    # Goal Mechanic
    public bool isPlayer1Goal;

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.gameObject.CompareTag("Ball"))
        {
            if(!isPlayer1Goal)
            {
                Debug.Log("Player 1 Scored");
                GameObject.Find("GameManager").GetComponent<GameManager>().Player1Scored();
            }
            else
            {
                Debug.Log("Player 2 Scored");
                GameObject.Find("GameManager").GetComponent<GameManager>().Player2Scored();
            }
        }
    }
    
    #Game Manager
        [Header("Ball")]
    public GameObject ball;

    [Header("Player 1")]
    public GameObject player1Paddle;
    public GameObject player1Goal;

    [Header("Player 2")]
    public GameObject player2Paddle;
    public GameObject player2Goal;

    [Header("Score UI")]
    public GameObject Player1Text;
    public GameObject Player2Text;

    [Header("Selected Skin")]
    public GameObject selectedSkin;
    public GameObject Player;
    private Sprite playerSprite; 

    private int Player1Score;
    private int Player2Score;


    void Start()
    {
        playerSprite = selectedSkin.GetComponent<SpriteRenderer>().sprite;
        Player.GetComponent<SpriteRenderer>().sprite = playerSprite;
        //PlayerPrefs.GetInt("skinIndex");
    }

    public void Player1Scored()
    {
        Player1Score++;
        Player1Text.GetComponent<TextMeshProUGUI>().text = Player1Score.ToString();
        ResetPosition();
        if(Player1Score == 5)
        {
            SceneManager.LoadScene("P1Wins");
        }
    }

    public void Player2Scored()
    {
        Player2Score++;
        Player2Text.GetComponent<TextMeshProUGUI>().text = Player2Score.ToString(); 
        ResetPosition();
        if(Player2Score == 5)
        {
            SceneManager.LoadScene("P2Wins");
        }
    }

    private void ResetPosition()
    {
        ball.GetComponent<Ball>().Reset();
        player1Paddle.GetComponent<Paddle>().Reset();
        player2Paddle.GetComponent<Paddle>().Reset();
    }
