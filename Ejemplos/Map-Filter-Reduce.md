using System;
using System.Linq;
using System.Collections.Generic;

//  VARIABLES

var myString = new List<string>() { "Casas", "Perro", "Ana", "Nene"};

// PROCEDIMIENTO

/*----map----*/

var intString = myString.Select(n => n.Length);

/*----filter----*/

var impares = intString.Where(n => n % 2 != 0);

/*----reduce----*/

var suma = impares.Aggregate(0, (sum, num) => sum + num);

// RESULTADO

var guardar = " ";
foreach (var item in intString) guardar += item + " "; // Guarda los elementos del array en un string
Console.WriteLine("Longitud de las palabras:" + guardar);

Console.WriteLine("Suma de la longitud de las palabras impares: " + suma);