using System;

var notas = new [] {7.5M, 4, 6, 5, 4, 6.5M, 7.5M}; 

// VARIABLES 

var suma = 0M;
var i = 0;
var media = 0.0M;

var aprobados = 0;
var porcentaje = 0.0M;

// PROCEDIMIENTO

calcularMedia();
calcularPorcentaje();

// RESULTADO

Console.WriteLine($"La media es: {media:0.00}");
Console.WriteLine($"El porcentaje de aprobado es: " + porcentaje + "%");

// FUNCIONES

void calcularMedia () {

    // VARIABLES

    i = 0;

    // PROCEDIMIENTO

    do {
        suma += notas[i];
        i++;
    } while (i < notas.Length);

    media = suma / notas.Length;

}

void calcularPorcentaje () {

    // VARIABLES

    i = 0;

    // PROCEDIMIENTO

    do {
        if (notas[i] >= 5) aprobados++; 
        i++;
    } while (i < notas.Length);

    porcentaje = (100 * aprobados) / notas.Length;

}