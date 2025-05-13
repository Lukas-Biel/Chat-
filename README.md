using UnityEngine;

public class GameManager : MonoBehaviour
{
    public static GameManager instance;
    public bool gameOver = false;

    void Awake()
    {
        instance = this;
    }

    public void EndGame()
    {
        gameOver = true;
        Debug.Log("Fim de jogo!");
    }
}

using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float mouseSensitivity = 100f;
    public Transform playerBody;
    public Camera playerCamera;

    float xRotation = 0f;

    void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
    }

    void Update()
    {
        Move();
        Look();
    }

    void Move()
    {
        float x = Input.GetAxis("Horizontal");
        float z = Input.GetAxis("Vertical");

        Vector3 move = transform.right * x + transform.forward * z;
        transform.position += move * moveSpeed * Time.deltaTime;
    }

    void Look()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity * Time.deltaTime;

        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -90f, 90f);

        playerCamera.transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        playerBody.Rotate(Vector3.up * mouseX);
    }
}

using UnityEngine;

public class Weapon : MonoBehaviour
{
    public GameObject bulletPrefab;
    public Transform firePoint;
    public float fireForce = 20f;

    void Update()
    {
        if (Input.GetButtonDown("Fire1"))
        {
            Shoot();
        }
    }

    void Shoot()
    {
        GameObject bullet = Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        Rigidbody rb = bullet.GetComponent<Rigidbody>();
        rb.AddForce(firePoint.forward * fireForce, ForceMode.Impulse);
    }
}

using UnityEngine;

public class ZoneController : MonoBehaviour
{
    public float shrinkRate = 0.5f;
    public float minSize = 5f;

    void Update()
    {
        if (transform.localScale.x > minSize)
        {
            transform.localScale -= Vector3.one * shrinkRate * Time.deltaTime;
        }
    }
}

using UnityEngine;

public class StatueManager : MonoBehaviour
{
    public GameObject[] statues;
    public Material[] topPlayerSkins;

    public void SetTopPlayers()
    {
        for (int i = 0; i < statues.Length && i < topPlayerSkins.Length; i++)
        {
            Renderer rend = statues[i].GetComponent<Renderer>();
            rend.material = topPlayerSkins[i];
        }
    }
}
