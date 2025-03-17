using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;  // Player movement speed
    private Vector2 moveDirection;  // Direction to move based on touch input

    void Update()
    {
        // Handle touch input (movement)
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            Vector2 touchPosition = Camera.main.ScreenToWorldPoint(touch.position);

            // Move the player towards the touch position
            moveDirection = (touchPosition - (Vector2)transform.position).normalized;
        }

        // Move the player in the calculated direction
        transform.Translate(moveDirection * moveSpeed * Time.deltaTime);
    }
}
using UnityEngine;

public class PlayerShooting : MonoBehaviour
{
    public GameObject bulletPrefab;  // Bullet prefab to instantiate
    public float bulletSpeed = 10f;  // Speed of the bullet
    public float shootingCooldown = 0.5f;  // Time between shots
    private float lastShotTime;  // Timer to track the shooting cooldown

    void Update()
    {
        // Shoot if a touch is detected on the right side of the screen
        if (Input.touchCount > 0 && Time.time > lastShotTime + shootingCooldown)
        {
            Touch touch = Input.GetTouch(0);

            // Check if the touch is on the right half of the screen
            if (touch.position.x > Screen.width / 2)
            {
                ShootBullet(touch.position);  // Shoot the bullet
                lastShotTime = Time.time;  // Reset the cooldown timer
            }
        }
    }

    void ShootBullet(Vector2 touchPosition)
    {
        // Instantiate a bullet
        GameObject bullet = Instantiate(bulletPrefab, transform.position, Quaternion.identity);

        // Get the Rigidbody2D component of the bullet
        Rigidbody2D rb = bullet.GetComponent<Rigidbody2D>();

        // Calculate the direction to shoot towards the touch position
        Vector2 shootingDirection = (Camera.main.ScreenToWorldPoint(touchPosition) - transform.position).normalized;

        // Apply velocity to the bullet
        rb.velocity = shootingDirection * bulletSpeed;
    }
}
using UnityEngine;

public class Bullet : MonoBehaviour
{
    public float lifespan = 3f;  // Time before bullet is destroyed

    void Start()
    {
        // Destroy the bullet after its lifespan
        Destroy(gameObject, lifespan);
    }

    void OnCollisionEnter2D(Collision2D collision)
    {
        // Destroy the bullet when it collides with anything
        Destroy(gameObject);
    }
}
