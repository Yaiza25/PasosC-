using System;

// VARIABLES

var sLuis = new sCalificacion("Luis", "H", 7.5M);
var sMarta = new sCalificacion("Marta", "M", 4);
var sMarcos = new sCalificacion("Marcos", "H", 6);
var sAroa = new sCalificacion("Aroa", "M", 5);
var sNerea = new sCalificacion("Nerea", "M", 4);
var sKike = new sCalificacion("Kike", "H", 6.5M);
var sJuan = new sCalificacion("Juan", "H", 7.5M);

var notas = new [] {sLuis, sMarta, sMarcos, sAroa, sNerea, sKike, sJuan};

var media = 0M;
var porcentaje = 0M;

// PROCEDIMIENTO

calcularMedia();
calcularPorcentaje();

// RESULTADO

Console.WriteLine($"La media es: {media:0.00}");
Console.WriteLine($"El porcentaje de aprobado es: {porcentaje:0.00}%");

// FUNCIONES

void calcularMedia() {

    // VARIABLES

    var suma = 0M;
    var i = 0;

    // PROCEDIMIENTO

    do {
        suma += notas[i].nota;
        i++;
    } while (i < notas.Length);

    media = suma / notas.Length;

}

void calcularPorcentaje() {

    // VARIABLES

    var aprobados = 0M;
    var i = 0;

    // PROCEDIMIENTO

    do {
        if (notas[i].nota >= 5) aprobados++; 
        i++;
    } while (i < notas.Length);

    porcentaje = (100 * aprobados) / notas.Length;

}

// ESTRCUTURA

public struct sCalificacion {

    public sCalificacion (String nombre, String genero, decimal nota) {
        this.nombre = nombre;
        this.genero = genero;
        this.nota = nota;
    }

    public String nombre { get; set;}
    public String genero { get; set;}
    public decimal nota { get; set;}

    public override string ToString() => $"({nombre}, {genero}, {nota}";

}