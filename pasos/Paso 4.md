using System;

var notas = new [] {7.5M, 4, 6, 5, 4, 6.5M, 7.5M}; 

// VARIABLES 

var media = 0.0M;
var porcentaje = 0.0M;

// RESULTADO

media = calcularMedia(notas);
porcentaje = calcularPorcentaje(notas);

Console.WriteLine($"La media es: {media:0.00}");
Console.WriteLine($"El porcentaje de aprobado es: " + porcentaje + "%");

// FUNCIONES

decimal calcularMedia (decimal[] datos) {

    // VARIABLES

    var suma = 0M;
    var media = 0M;
    var i = 0;

    // PROCEDIMIENTO

    do {
        suma += datos[i];
        i++;
    } while (i < datos.Length);

    media = suma / datos.Length;

    // RESULTADO

    return media;

}

decimal calcularPorcentaje (decimal[] datos) {

    // VARIABLES

    var aprobados = 0;
    var porcentaje = 0.0M;
    var i = 0;

    // PROCEDIMIENTO

    do {
        if (datos[i] >= 5) aprobados++; 
        i++;
    } while (i < datos.Length);

    porcentaje = (100 * aprobados) / datos.Length;

    // RESULTADO

    return porcentaje;

}