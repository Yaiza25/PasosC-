using System;

// VARIABLES

var reLuis = new reCalificacion("Luis", "H", 7.5M);
var reMarta = new reCalificacion("Marta", "M", 4);
var reMarcos = new reCalificacion("Marcos", "H", 6);
var reAroa = new reCalificacion("Aroa", "M", 5);
var reNerea = new reCalificacion("Nerea", "M", 4);
var reKike = new reCalificacion("Kike", "H", 6.5M);
var reJuan = new reCalificacion("Juan", "H", 7.5M);

var notas = new [] {reLuis, reMarta, reMarcos, reAroa, reNerea, reKike, reJuan};

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

// RECORDS

public record reCalificacion (String nombre, String genero, decimal nota);