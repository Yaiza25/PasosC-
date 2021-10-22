using System;

//  VARIABLES

var sLuis = new calificacion("Luis", "H", 7.5M);
var sMarta = new calificacion("Marta", "M", 4);
var sMarcos = new calificacion("Marcos", "H", 6);
var sAroa = new calificacion("Aroa", "M", 5);
var sNerea = new calificacion("Nerea", "M", 4);
var sKike = new calificacion("Kike", "H", 6.5M);
var sJuan = new calificacion("Juan", "H", 7.5M);

var notas = new [] {sLuis, sMarta, sMarcos, sAroa, sNerea, sKike, sJuan};

var sistema = new sistema(notas);

// RESULTADO

Console.WriteLine($"La media es: {sistema.calcularMedia():0.00}");

//  CONSTRUCTOR 

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

}