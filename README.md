using UnityEngine;

public class FurnitureItem : MonoBehaviour
{
    private bool isDragging = false;
    private Vector3 offset;
    private float rotationSpeed = 90f; // Grados por segundo

    // Se llama cuando el jugador hace clic en el objeto
    private void OnMouseDown()
    {
        // Calculamos la distancia entre el objeto y la cámara para el arrastre
        offset = gameObject.transform.position - Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.WorldToScreenPoint(gameObject.transform.position).z));
        isDragging = true;
    }

    // Se llama mientras el jugador arrastra el objeto
    private void OnMouseDrag()
    {
        if (isDragging)
        {
            // Movemos el objeto siguiendo el ratón, manteniendo su altura original (eje Y)
            Vector3 curScreenPoint = new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.WorldToScreenPoint(gameObject.transform.position).z);
            Vector3 curPosition = Camera.main.ScreenToWorldPoint(curScreenPoint) + offset;
            curPosition.y = gameObject.transform.position.y; // Mantenemos la altura
            transform.position = curPosition;
        }
    }

    // Se llama cuando el jugador suelta el botón del ratón
    private void OnMouseUp()
    {
        isDragging = false;
        // Aquí podrías añadir la lógica para "snapear" a la cuadrícula
        SnapToGrid();
    }

    // Lógica para alinear el objeto a una cuadrícula invisible
    void SnapToGrid()
    {
        float gridSize = 0.5f; // Tamaño de cada celda de la cuadrícula
        transform.position = new Vector3(
            Mathf.Round(transform.position.x / gridSize) * gridSize,
            transform.position.y, // No snapeamos la altura
            Mathf.Round(transform.position.z / gridSize) * gridSize
        );
    }

    // Usamos las teclas Q/E para rotar el objeto cuando está seleccionado
    private void Update()
    {
        if (isDragging)
        {
            if (Input.GetKey(KeyCode.Q))
            {
                transform.Rotate(Vector3.up, -rotationSpeed * Time.deltaTime);
            }
            if (Input.GetKey(KeyCode.E))
            {
                transform.Rotate(Vector3.up, rotationSpeed * Time.deltaTime);
            }
        }
    }
}
