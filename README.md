# Ejemplos-de-patrones

# Singleton

```C#
using System;
using System.Collections.Generic;

public class VentaCarrosSingleton
{
    // La única instancia de la clase (Singleton)
    private static VentaCarrosSingleton instancia;

    // Lista que almacena las ventas de carros
    private List<string> ventasDeCarros;

    // Constructor privado para evitar la instanciación directa
    private VentaCarrosSingleton()
    {
        ventasDeCarros = new List<string>();
    }

    // Método estático para obtener la única instancia de la clase
    public static VentaCarrosSingleton Instancia
    {
        get
        {
            if (instancia == null)
            {
                instancia = new VentaCarrosSingleton();
            }
            return instancia;
        }
    }

    // Método para registrar una venta de carro
    public void RegistrarVenta(string carro)
    {
        ventasDeCarros.Add(carro);
        Console.WriteLine($"Se ha registrado la venta del carro: {carro}");
    }

    // Método para mostrar todas las ventas realizadas
    public void MostrarVentas()
    {
        Console.WriteLine("Ventas de Carros Realizadas:");
        foreach (var venta in ventasDeCarros)
        {
            Console.WriteLine(venta);
        }
    }
}

public class ProgramaVentaCarros
{
    public static void Main()
    {
        // Obtener la instancia del Singleton
        VentaCarrosSingleton ventasCarros = VentaCarrosSingleton.Instancia;

        // Registrar ventas de carros
        ventasCarros.RegistrarVenta("Toyota Corolla");
        ventasCarros.RegistrarVenta("Honda Civic");
        ventasCarros.RegistrarVenta("Ford Mustang");

        // Mostrar todas las ventas realizadas
        ventasCarros.MostrarVentas();
    }
}
```

Explicación:

    Singleton (VentaCarrosSingleton): Se asegura de que solo haya una única instancia que maneje las ventas de carros. Esto se logra usando un constructor privado y un método estático para acceder a la instancia.
    Registro de Ventas: Puedes registrar las ventas de los autos usando el método RegistrarVenta().
    Mostrar Ventas: El método MostrarVentas() imprime todas las ventas registradas hasta el momento.
