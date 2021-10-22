using System;
using System.IO;
using System.Linq;
using System.Collections.Generic;

// VARIABLES

var repositorio = new RepositorioCSV();
var sistema = new Sistema(repositorio);
var vista = new Vista();
var controlador = new Controlador(sistema, vista);

// PROCEDIMIENTO

controlador.run();

// VISTA - Interfaz que ve el usuario

public class Vista {

    public void mostrarOpciones() {

        Console.WriteLine("========= Menu =========\n");

        Console.WriteLine("1.- Realizar media\n" + 
                          "2.- Obtener la mejor nota\n" +
                          "3.- Obtener porcentaje de aprobados\n");
    }

    public int obtenerOpcion() {

        int opcion = 0;
        bool entradaIncorrecta = true;

        while (entradaIncorrecta) {

            try {
                Console.Write("Seleciona una opción: ");
                opcion = int.Parse(Console.ReadLine());
                if (opcion == 1 || opcion == 2 || opcion == 3) entradaIncorrecta = false;
            } catch (System.Exception) {
                ;
            }

        }

        return opcion;

    }

}

// SISTEMA - Operaciones con los datos

public class Sistema {

    IRepositorio Repositorio;

    List<Calificacion> Notas;

    public Sistema(IRepositorio repositorio) {
        Repositorio = repositorio;
        Repositorio.Inicializar();
    }

    // FUNCIONES - Media

    private decimal calcularSuma(decimal[] datos) => datos.Sum();

    public decimal calcularMedia() {

        Notas = Repositorio.CargarCalificaciones();

        var notas = Notas.Select(n => n.Nota).ToArray();
        return calcularSuma(notas) / Notas.Count;

    }

    // FUNCIONES - Porcentaje de aprobados

    public decimal calcularPorcentaje () {

        var aprobados = 0M;
        Notas = Repositorio.CargarCalificaciones();

        for (int i = 0; i < Notas.Count; i++) {
            if (Notas[i].Nota >= 5) aprobados++; 
        }

        return (100 * aprobados) / Notas.Count;

    }

    // FUNCIONES - Mejor nota

    public decimal notaMasAlta() => Repositorio.CargarCalificaciones().Max(n => n.Nota);

}

// CONTROLADOR - Enlace entre el sistema y la vista

public class Controlador {

    private Sistema sistema;
    private Vista vista;

    public Controlador(Sistema sistema, Vista vista) {
        this.sistema = sistema;
        this.vista = vista;
    }

    public void run() {

        Boolean entrada = true;

        while (entrada) {

            Console.Clear();

            vista.mostrarOpciones();
            int opcion = vista.obtenerOpcion();

            switch(opcion) {

                case 1:
                    Console.WriteLine($"\n\nLa media es: {sistema.calcularMedia():0.00}");
                break;

                case 2:
                    Console.WriteLine("\n\nLa nota mas alta es: " + sistema.notaMasAlta());
                break;

                case 3:
                    Console.WriteLine($"\n\nEl porcentaje de aprobados es: {sistema.calcularPorcentaje():0.00}%");
                break;

            }

            Console.WriteLine("\n\nEscriba SI para continuar y NO para finalizar");
            var confirmacion = Console.ReadLine();

            while (confirmacion.ToLower() != "no" && confirmacion.ToLower() != "si") {
                Console.WriteLine("\nEscriba SI para continuar y NO para finalizar");
                confirmacion = Console.ReadLine();
            }

            if (confirmacion.ToLower() == "no") entrada = false;       

        }

    }
    
}

//  OBJETO CALIFICACIONES - Constructor

public class Calificacion {

    public string Nombre;
    public string Genero;
    public decimal Nota;

    public override string ToString() => $"({Nombre}, {Genero}, {Nota}";
    
    internal static Calificacion analizarFila(string frase) {

        var columnas = frase.Split(',');
        return new Calificacion() {Nombre = columnas[0], Genero = columnas[1], Nota = decimal.Parse(columnas[2].Replace('.', ','))};

    }

}

// REPOSITORIO - Datos guardados

public interface IRepositorio {
    void Inicializar();
    List<Calificacion> CargarCalificaciones();

}

public class RepositorioCSV : IRepositorio {
    
    string fichero;

    void IRepositorio.Inicializar() {
        this.fichero = "notas.csv";
    }
    List<Calificacion> IRepositorio.CargarCalificaciones() {

        return File.ReadAllLines(fichero).Skip(1).Where(fila => fila.Length > 0).Select(Calificacion.analizarFila).ToList();

    }

}