using System;

var notas = new [] {7.5M, 4, 6, 5, 4, 6.5M, 7.5M};
var i = 0;  

/*
Con las siguientes instrucciones IF, GOTO y LABEL calcular la media del array y el porcentaje de aprobados
- Todo variables globales
- Ninguna funcion
*/

// VARIABLES 

var suma = 0.0M;
var media = 0.0M;

var aprobados = 0;
var porcentaje = 0.0M;

// MARCADOR

aqui:

// CICLO

if (notas[i] >= 5) aprobados++; 

suma += notas[i];
i++;
if (i < notas.Length) goto aqui;

media = suma / notas.Length;
porcentaje = (100 * aprobados) / notas.Length;

// RESULTADO

Console.WriteLine($"La media es: {media:0.00}");
Console.WriteLine($"El porcentaje de aprobado es: " + porcentaje + "%");