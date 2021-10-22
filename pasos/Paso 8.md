using System;
using System.Linq;

// VARIABLES

var cLuis = new calificacion("Luis", "H", 7.5M);
var cMarta = new calificacion("Marta", "M", 4);
var cMarcos = new calificacion("Marcos", "H", 6);
var cAroa = new calificacion("Aroa", "M", 5);
var cNerea = new calificacion("Nerea", "M", 4);
var cKike = new calificacion("Kike", "H", 6.5M);
var cJuan = new calificacion("Juan", "H", 7.5M);

var notas = new [] {cLuis, cMarta, cMarcos, cAroa, cNerea, cKike, cJuan};

var sistema = new Sistema(notas);
var vista = new Vista();
var controlador = new Controlador(sistema, vista);

// PROCEDIMIENTO

controlador.run();

// VISTA - Interfaz que ve el usuario

public class Vista {

    public void mostrarOpciones() {

        Console.WriteLine("========= Menu =========" + "\n");

        Console.WriteLine("1.- Realizar media" + "\n" + 
                          "2.- Obtener la mejor nota" + "\n" +
                          "3.- Obtener porcentaje de aprobados" + "\n");
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

    calificacion[] notas;

    public Sistema(calificacion[] notas) {
        this.notas = notas;
    }

    // FUNCIONES - Media

    private decimal calcularSuma(decimal[] datos) {return datos.Sum();}

    public decimal calcularMedia() {

        var notas2 = notas.Select(n => n.Nota).ToArray();
        return calcularSuma(notas2) / notas2.Length;

    }

    // FUNCIONES - Porcentaje de aprobados

    public decimal calcularPorcentaje () {

        var aprobados = 0M;
        var i = 0;
        var porcentaje = 0M;

        do {
            if (notas[i].Nota >= 5) aprobados++; 
            i++;
        } while (i < notas.Length);

        porcentaje = (100 * aprobados) / notas.Length;

        return porcentaje;

    }

    // FUNCIONES - Mejor nota

    public decimal notaMasAlta() {

        decimal notaAlta = 0;

        for (int i = 0; i < notas.Length; i++) {
            if (notaAlta < notas[i].Nota) notaAlta = notas[i].Nota;
        }

        return notaAlta;

    }

}

// CONTROLADOR - Enlace entre el sistema y la vista

public class Controlador {

    private Sistema sistema;
    private Vista vista;

    public Controlador(Sistema sistema, Vista vista)
    {
        this.sistema = sistema;
        this.vista = vista;
    }

    public void run() {

        Console.Clear();

        vista.mostrarOpciones();
        int opcion = vista.obtenerOpcion();

        switch(opcion) {

            case 1:
                Console.WriteLine("");
                Console.WriteLine($"La media es: {sistema.calcularMedia():0.00}");
            break;

            case 2:
                Console.WriteLine("");
                Console.WriteLine("La nota mas alta es: " + sistema.notaMasAlta());
            break;

            case 3:
                Console.WriteLine("");
                Console.WriteLine($"El porcentaje de aprobados es: {sistema.calcularPorcentaje():0.00}%");
            break;
        }

    }
    
}

//  OBJETO CALIFICACIONES - Constructor

public class calificacion {

    public string Nombre;
    public string Genero;
    public decimal Nota;

    public calificacion (String nombre, String genero, decimal nota) {
        Nombre = nombre;
        Genero = genero;
        Nota = nota;
    }

    public override string ToString() => $"({Nombre}, {Genero}, {Nota}";
    
}