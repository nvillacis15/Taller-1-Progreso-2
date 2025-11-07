#include <stdio.h>
#include <string.h>

#define MAX 10
#define MAX_NOMBRE 30

// ==== PROTOTIPOS DE FUNCIONES ====
void ingresarDatos(char nombres[][MAX_NOMBRE], float precios[], int n);
float calcularTotal(float precios[], int n);
float calcularPromedio(float precios[], int n);
int indiceMasCaro(float precios[], int n);
int indiceMasBarato(float precios[], int n);
int buscarProducto(char nombres[][MAX_NOMBRE], float precios[], int n, char buscado[]);

// ==== FUNCIONES ====
void ingresarDatos(char nombres[][MAX_NOMBRE], float precios[], int n) {
    for (int i = 0; i < n; i++) {
        printf("Ingrese el nombre del producto %d: ", i + 1);
        scanf(" %[^\n]", nombres[i]);
        printf("Ingrese el precio del producto %d: ", i + 1);
        scanf("%f", &precios[i]);
    }
}

float calcularTotal(float precios[], int n) {
    float total = 0;
    for (int i = 0; i < n; i++) {
        total += precios[i];
    }
    return total;
}

float calcularPromedio(float precios[], int n) {
    return calcularTotal(precios, n) / n;
}

int indiceMasCaro(float precios[], int n) {
    int indice = 0;
    for (int i = 1; i < n; i++) {
        if (precios[i] > precios[indice])
            indice = i;
    }
    return indice;
}

int indiceMasBarato(float precios[], int n) {
    int indice = 0;
    for (int i = 1; i < n; i++) {
        if (precios[i] < precios[indice])
            indice = i;
    }
    return indice;
}

int buscarProducto(char nombres[][MAX_NOMBRE], float precios[], int n, char buscado[]) {
    for (int i = 0; i < n; i++) {
        if (strcmp(nombres[i], buscado) == 0)
            return i;
    }
    return -1;
}

// ==== PROGRAMA PRINCIPAL ====
int main() {
    char nombres[MAX][MAX_NOMBRE];
    float precios[MAX];
    int n;

    printf("Ingrese el numero de productos (maximo %d): ", MAX);
    scanf("%d", &n);

    if (n < 1 || n > MAX) {
        printf("Numero invalido de productos.\n");
        return 1;
    }

    ingresarDatos(nombres, precios, n);

    float total = calcularTotal(precios, n);
    float promedio = calcularPromedio(precios, n);
    int iCaro = indiceMasCaro(precios, n);
    int iBarato = indiceMasBarato(precios, n);

    printf("\n===== RESULTADOS =====\n");
    printf("Precio total del inventario: %.2f\n", total);
    printf("Precio promedio: %.2f\n", promedio);
    printf("Producto mas caro: %s (%.2f)\n", nombres[iCaro], precios[iCaro]);
    printf("Producto mas barato: %s (%.2f)\n", nombres[iBarato], precios[iBarato]);

    char buscado[MAX_NOMBRE];
    printf("\nIngrese el nombre de un producto para buscar su precio: ");
    scanf(" %[^\n]", buscado);

    int indice = buscarProducto(nombres, precios, n, buscado);
    if (indice != -1)
        printf("El producto %s tiene un precio de %.2f\n", nombres[indice], precios[indice]);
    else
        printf("Producto no encontrado.\n");

    return 0;
}
