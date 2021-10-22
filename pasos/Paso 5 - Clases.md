using System;

//  VARIABLES

var cLuis = new calificacion("Luis", "H", 7.5M);
var cMarta = new calificacion("Marta", "M", 4);
var cMarcos = new calificacion("Marcos", "H", 6);
var cAroa = new calificacion("Aroa", "M", 5);
var cNerea = new calificacion("Nerea", "M", 4);
var cKike = new calificacion("Kike", "H", 6.5M);
var cJuan = new calificacion("Juan", "H", 7.5M);

var notas = new [] {cLuis, cMarta, cMarcos, cAroa, cNerea, cKike, cJuan};

var sistema = new sistema(notas);

// RESULTADO

Console.WriteLine($"La media es: {sistema.calcularMedia():0.00}");
Console.WriteLine($"El porcentaje es: {sistema.calcularPorcentaje():0.00}%");

//  CLASES

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

public class sistema {

    public calificacion[] notas;

    public sistema (calificacion[] notas) {
        this.notas = notas;
    }

    // FUNCIONES

    private decimal calcularSuma (calificacion[] datos) {

        // VARIABLES

        decimal suma = 0M;
        int i = 0;

        // PROCEDIMIENTO

        do {
            suma += datos[i].Nota;
            i++;
        } while (i < datos.Length);

        // DEVOLVER

        return suma;

    }

    public decimal calcularMedia () {

        // VARIABLES

        var suma = calcularSuma(notas);
        decimal media = 0M;

        // PROCEDIMIENTO

        media = suma / notas.Length;

        // DEVOLVER

        return media;

    }

    public decimal calcularPorcentaje () {

        // VARIABLES

        var aprobados = 0M;
        var i = 0;
        var porcentaje = 0M;

        // PROCEDIMIENTO

        do {
            if (notas[i].Nota >= 5) aprobados++; 
            i++;
        } while (i < notas.Length);

        porcentaje = (100 * aprobados) / notas.Length;

        // DEVOLVER

        return porcentaje;

    }

}