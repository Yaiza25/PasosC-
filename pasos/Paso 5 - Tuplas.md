using System;

// VARIABLES

var tLuis = (nombre: "Luis", genero: "H", nota: 7.5M);
var tMarta = (nombre: "Marta", genero: "M", nota: 4);
var tMarcos = (nombre: "Marcos", genero: "H", nota: 6);
var tAroa = (nombre: "Aroa", genero: "M", nota: 5);
var tNerea = (nombre: "Nerea", genero: "M", nota: 4);
var tKike = (nombre: "Kike", genero: "H", nota: 6.5M);
var tJuan = (nombre: "Juan", genero: "H", nota: 7.5M);

var notas = new [] {tLuis, tMarta, tMarcos, tAroa, tNerea, tKike, tJuan};

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