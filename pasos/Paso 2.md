using System;

// VARIABLES 

var notas = new [] {7.5M, 4, 6, 5, 4, 6.5M, 7.5M};
var i = 0;  

var suma = 0.0M;
var media = 0.0M;

var aprobados = 0;
var porcentaje = 0.0M;

// PROCEDIMIENTO

for (i = 0; i < notas.Length; i++) {
    if (notas[i] >= 5) aprobados++; 
    suma += notas[i];
}

media = suma / notas.Length;
porcentaje = (100 * aprobados) / notas.Length;

// RESULTADO

Console.WriteLine($"La media es: {media:0.00}");
Console.WriteLine($"El porcentaje de aprobado es: " + porcentaje + "%");