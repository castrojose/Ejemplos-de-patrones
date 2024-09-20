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

# Factory Method

```C#
    using System;

// Producto abstracto (Auto)
public abstract class Auto
{
    public abstract void MostrarDetalles();
}

// Productos concretos (diferentes tipos de autos)
public class ToyotaCorolla : Auto
{
    public override void MostrarDetalles()
    {
        Console.WriteLine("Auto: Toyota Corolla, Año: 2024, Precio: $20,000");
    }
}

public class HondaCivic : Auto
{
    public override void MostrarDetalles()
    {
        Console.WriteLine("Auto: Honda Civic, Año: 2024, Precio: $22,000");
    }
}

public class FordMustang : Auto
{
    public override void MostrarDetalles()
    {
        Console.WriteLine("Auto: Ford Mustang, Año: 2024, Precio: $35,000");
    }
}

// Creador abstracto (Tienda de Carros)
public abstract class TiendaCarros
{
    // Método Factory que las subclases implementarán
    public abstract Auto CrearAuto(string modelo);

    // Método para realizar la venta
    public void VenderAuto(string modelo)
    {
        Auto auto = CrearAuto(modelo);
        Console.WriteLine("Venta realizada:");
        auto.MostrarDetalles();
    }
}

// Creador concreto (TiendaEspecifica que implementa el método Factory)
public class TiendaEspecifica : TiendaCarros
{
    public override Auto CrearAuto(string modelo)
    {
        switch (modelo)
        {
            case "Toyota Corolla":
                return new ToyotaCorolla();
            case "Honda Civic":
                return new HondaCivic();
            case "Ford Mustang":
                return new FordMustang();
            default:
                throw new ArgumentException("Modelo de auto no disponible.");
        }
    }
}

public class ProgramaVentaCarros
{
    public static void Main()
    {
        // Crear una tienda específica
        TiendaCarros tienda = new TiendaEspecifica();

        // Vender autos
        tienda.VenderAuto("Toyota Corolla");
        tienda.VenderAuto("Honda Civic");
        tienda.VenderAuto("Ford Mustang");
    }
}
```
Explicación:

Auto (Producto Abstracto): Define una interfaz para los objetos que la fábrica va a crear, en este caso, un auto con un método MostrarDetalles().
ToyotaCorolla, HondaCivic, FordMustang (Productos Concretos): Clases concretas que heredan de Auto y representan diferentes tipos de autos que se pueden vender.    
TiendaCarros (Creador Abstracto): Define el método Factory CrearAuto que las subclases deben implementar para crear diferentes tipos de autos.
TiendaEspecifica (Creador Concreto): Implementa el método Factory para decidir qué tipo de auto se crea dependiendo del modelo solicitado.
Método VenderAuto(): En este método se usa el Factory Method para crear el tipo de auto adecuado y luego mostrar los detalles de la venta.

# Abstract Factory

```C#
    using System;

// Productos abstractos (Partes del Auto)
public abstract class Auto
{
    public abstract void MostrarDetalles();
}

public abstract class Motor
{
    public abstract void MostrarDetallesMotor();
}

public abstract class Transmision
{
    public abstract void MostrarDetallesTransmision();
}

// Productos concretos (Autos Toyota y sus componentes)
public class ToyotaCorolla : Auto
{
    public override void MostrarDetalles()
    {
        Console.WriteLine("Auto: Toyota Corolla, Año: 2024, Precio: $20,000");
    }
}

public class MotorToyota : Motor
{
    public override void MostrarDetallesMotor()
    {
        Console.WriteLine("Motor Toyota: 1.8L, 4 cilindros");
    }
}

public class TransmisionToyota : Transmision
{
    public override void MostrarDetallesTransmision()
    {
        Console.WriteLine("Transmisión Toyota: Automática CVT");
    }
}

// Productos concretos (Autos Honda y sus componentes)
public class HondaCivic : Auto
{
    public override void MostrarDetalles()
    {
        Console.WriteLine("Auto: Honda Civic, Año: 2024, Precio: $22,000");
    }
}

public class MotorHonda : Motor
{
    public override void MostrarDetallesMotor()
    {
        Console.WriteLine("Motor Honda: 2.0L, 4 cilindros");
    }
}

public class TransmisionHonda : Transmision
{
    public override void MostrarDetallesTransmision()
    {
        Console.WriteLine("Transmisión Honda: Manual 6 velocidades");
    }
}

// Fábrica abstracta
public abstract class FabricaAuto
{
    public abstract Auto CrearAuto();
    public abstract Motor CrearMotor();
    public abstract Transmision CrearTransmision();
}

// Fábrica concreta para autos Toyota
public class FabricaToyota : FabricaAuto
{
    public override Auto CrearAuto()
    {
        return new ToyotaCorolla();
    }

    public override Motor CrearMotor()
    {
        return new MotorToyota();
    }

    public override Transmision CrearTransmision()
    {
        return new TransmisionToyota();
    }
}

// Fábrica concreta para autos Honda
public class FabricaHonda : FabricaAuto
{
    public override Auto CrearAuto()
    {
        return new HondaCivic();
    }

    public override Motor CrearMotor()
    {
        return new MotorHonda();
    }

    public override Transmision CrearTransmision()
    {
        return new TransmisionHonda();
    }
}

// Cliente
public class TiendaCarros
{
    private Auto auto;
    private Motor motor;
    private Transmision transmision;

    public TiendaCarros(FabricaAuto fabrica)
    {
        auto = fabrica.CrearAuto();
        motor = fabrica.CrearMotor();
        transmision = fabrica.CrearTransmision();
    }

    public void MostrarDetalles()
    {
        auto.MostrarDetalles();
        motor.MostrarDetallesMotor();
        transmision.MostrarDetallesTransmision();
    }
}

public class ProgramaVentaCarros
{
    public static void Main()
    {
        // Crear fábrica Toyota
        FabricaAuto fabricaToyota = new FabricaToyota();
        TiendaCarros tiendaToyota = new TiendaCarros(fabricaToyota);
        Console.WriteLine("Detalles de la venta del Toyota:");
        tiendaToyota.MostrarDetalles();

        Console.WriteLine();

        // Crear fábrica Honda
        FabricaAuto fabricaHonda = new FabricaHonda();
        TiendaCarros tiendaHonda = new TiendaCarros(fabricaHonda);
        Console.WriteLine("Detalles de la venta del Honda:");
        tiendaHonda.MostrarDetalles();
    }
}
```
Explicación:

 Auto, Motor, Transmision (Productos Abstractos): Estas son las clases abstractas que representan los diferentes componentes de los autos.
ToyotaCorolla, HondaCivic, MotorToyota, MotorHonda, TransmisionToyota, TransmisionHonda (Productos Concretos): Estas clases concretas implementan las interfaces abstractas y
definen autos y componentes específicos de cada marca.
FabricaAuto (Fábrica Abstracta): Define la interfaz para crear los productos relacionados (Auto, Motor, Transmision).
FabricaToyota y FabricaHonda (Fábricas Concretas): Implementan la fábrica abstracta para crear autos y sus componentes correspondientes para cada marca.
TiendaCarros (Cliente): Esta clase recibe una fábrica concreta (Toyota o Honda) y usa los métodos de la fábrica para crear un auto y sus componentes.

# Builder
```C#
    using System;

// Producto (Auto)
public class Auto
{
    public string Modelo { get; set; }
    public string Motor { get; set; }
    public string Transmision { get; set; }
    public string Color { get; set; }

    public void MostrarDetalles()
    {
        Console.WriteLine($"Modelo: {Modelo}");
        Console.WriteLine($"Motor: {Motor}");
        Console.WriteLine($"Transmisión: {Transmision}");
        Console.WriteLine($"Color: {Color}");
    }
}

// Builder abstracto
public abstract class AutoBuilder
{
    protected Auto auto;

    public Auto ObtenerAuto()
    {
        return auto;
    }

    public void CrearNuevoAuto()
    {
        auto = new Auto();
    }

    public abstract void ConstruirModelo();
    public abstract void ConstruirMotor();
    public abstract void ConstruirTransmision();
    public abstract void ConstruirColor();
}

// Builder concreto para construir un Toyota Corolla
public class ToyotaCorollaBuilder : AutoBuilder
{
    public override void ConstruirModelo()
    {
        auto.Modelo = "Toyota Corolla";
    }

    public override void ConstruirMotor()
    {
        auto.Motor = "1.8L 4 cilindros";
    }

    public override void ConstruirTransmision()
    {
        auto.Transmision = "Automática CVT";
    }

    public override void ConstruirColor()
    {
        auto.Color = "Blanco";
    }
}

// Builder concreto para construir un Honda Civic
public class HondaCivicBuilder : AutoBuilder
{
    public override void ConstruirModelo()
    {
        auto.Modelo = "Honda Civic";
    }

    public override void ConstruirMotor()
    {
        auto.Motor = "2.0L 4 cilindros";
    }

    public override void ConstruirTransmision()
    {
        auto.Transmision = "Manual 6 velocidades";
    }

    public override void ConstruirColor()
    {
        auto.Color = "Negro";
    }
}

// Director (el que controla el proceso de construcción)
public class Director
{
    private AutoBuilder _autoBuilder;

    public void EstablecerBuilder(AutoBuilder autoBuilder)
    {
        _autoBuilder = autoBuilder;
    }

    public Auto ConstruirAuto()
    {
        _autoBuilder.CrearNuevoAuto();
        _autoBuilder.ConstruirModelo();
        _autoBuilder.ConstruirMotor();
        _autoBuilder.ConstruirTransmision();
        _autoBuilder.ConstruirColor();
        return _autoBuilder.ObtenerAuto();
    }
}

// Cliente
public class ProgramaVentaCarros
{
    public static void Main()
    {
        // Crear el director
        Director director = new Director();

        // Crear y construir un Toyota Corolla
        AutoBuilder toyotaBuilder = new ToyotaCorollaBuilder();
        director.EstablecerBuilder(toyotaBuilder);
        Auto toyota = director.ConstruirAuto();
        Console.WriteLine("Detalles del Toyota Corolla:");
        toyota.MostrarDetalles();

        Console.WriteLine();

        // Crear y construir un Honda Civic
        AutoBuilder hondaBuilder = new HondaCivicBuilder();
        director.EstablecerBuilder(hondaBuilder);
        Auto honda = director.ConstruirAuto();
        Console.WriteLine("Detalles del Honda Civic:");
        honda.MostrarDetalles();
    }
}
```
Explicación:
Auto (Producto): Esta es la clase del objeto que se va a construir. Tiene propiedades como el modelo, motor, transmisión y color. El método MostrarDetalles() imprime los detalles del auto.
AutoBuilder (Builder Abstracto): Define los métodos abstractos que los "builders" concretos deben implementar. También tiene un método ObtenerAuto() para obtener el auto construido.
ToyotaCorollaBuilder y HondaCivicBuilder (Builders Concretos): Implementan el proceso de construcción para los autos específicos, estableciendo el modelo, motor, transmisión y color.
Director: Controla el proceso de construcción. Llama a los métodos del builder concreto en el orden correcto para construir el auto.
ProgramaVentaCarros (Cliente): Es el cliente que usa el director para construir diferentes autos (en este caso, un Toyota Corolla y un Honda Civic).

# Prototype

```C#
    using System;

// Clase Auto que implementa la interfaz ICloneable para habilitar la clonación
public class Auto : ICloneable
{
    public string Modelo { get; set; }
    public string Motor { get; set; }
    public string Transmision { get; set; }
    public string Color { get; set; }

    public Auto(string modelo, string motor, string transmision, string color)
    {
        Modelo = modelo;
        Motor = motor;
        Transmision = transmision;
        Color = color;
    }

    // Implementación del método Clone
    public object Clone()
    {
        // Crea una copia superficial del objeto Auto
        return this.MemberwiseClone();
    }

    public void MostrarDetalles()
    {
        Console.WriteLine($"Modelo: {Modelo}");
        Console.WriteLine($"Motor: {Motor}");
        Console.WriteLine($"Transmisión: {Transmision}");
        Console.WriteLine($"Color: {Color}");
    }
}

// Prototipos concretos para autos específicos
public class PrototipoAutos
{
    private Auto _toyotaCorolla;
    private Auto _hondaCivic;

    public PrototipoAutos()
    {
        // Creación de prototipos iniciales
        _toyotaCorolla = new Auto("Toyota Corolla", "1.8L 4 cilindros", "Automática CVT", "Blanco");
        _hondaCivic = new Auto("Honda Civic", "2.0L 4 cilindros", "Manual 6 velocidades", "Negro");
    }

    // Obtener una copia del prototipo del Toyota Corolla
    public Auto ObtenerPrototipoToyota()
    {
        return (Auto)_toyotaCorolla.Clone();
    }

    // Obtener una copia del prototipo del Honda Civic
    public Auto ObtenerPrototipoHonda()
    {
        return (Auto)_hondaCivic.Clone();
    }
}

// Cliente
public class ProgramaVentaCarros
{
    public static void Main()
    {
        // Crear el almacén de prototipos
        PrototipoAutos prototipoAutos = new PrototipoAutos();

        // Clonar y modificar un Toyota Corolla
        Auto toyota1 = prototipoAutos.ObtenerPrototipoToyota();
        toyota1.Color = "Rojo";  // Cambiar color del clon
        Console.WriteLine("Detalles del Toyota Corolla (clon modificado):");
        toyota1.MostrarDetalles();

        Console.WriteLine();

        // Clonar y modificar un Honda Civic
        Auto honda1 = prototipoAutos.ObtenerPrototipoHonda();
        honda1.Motor = "1.5L Turbo";  // Cambiar motor del clon
        Console.WriteLine("Detalles del Honda Civic (clon modificado):");
        honda1.MostrarDetalles();

        Console.WriteLine();

        // Clonar otro Toyota Corolla sin modificar
        Auto toyota2 = prototipoAutos.ObtenerPrototipoToyota();
        Console.WriteLine("Detalles del Toyota Corolla (otro clon):");
        toyota2.MostrarDetalles();
    }
}
```

Explicación:

Auto (Producto): La clase Auto representa el objeto que queremos clonar. Implementa la interfaz ICloneable y tiene un método Clone() que realiza una clonación superficial del objeto usando MemberwiseClone().
PrototipoAutos (Clase que maneja prototipos): Esta clase tiene instancias iniciales de autos (un Toyota Corolla y un Honda Civic), y expone métodos para obtener clones de estos prototipos.
Clonación y Personalización: Cuando llamas a ObtenerPrototipoToyota() o ObtenerPrototipoHonda(), obtienes una copia del prototipo original. Luego, puedes modificar las propiedades del clon sin afectar al prototipo original.
Cliente (ProgramaVentaCarros): El cliente usa los prototipos para clonar autos, personalizarlos y mostrarlos. Cada clon puede ser modificado de forma independiente sin afectar los prototipos originales.
