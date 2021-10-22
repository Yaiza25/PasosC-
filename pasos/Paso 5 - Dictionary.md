using System;
using System.Collections.Generic;

// VARIABLES

var dLuis = new Dictionary<string, Object>();
dLuis["nombre"] = "Luis";
dLuis["genero"] = "H";
dLuis["nota"] = 7.5M;

var dMarta = new Dictionary<string, Object>();
dMarta["nombre"] = "Marta";
dMarta["genero"] = "M";
dMarta["nota"] = 4;

var dMarcos = new Dictionary<string, Object>();
dMarcos["nombre"] = "Marcos";
dMarcos["genero"] = "H";
dMarcos["nota"] = 6;

var dAroa = new Dictionary<string, Object>();
dAroa["nombre"] = "Aroa";
dAroa["genero"] = "M";
dAroa["nota"] = 5;

var dNerea = new Dictionary<string, Object>();
dNerea["nombre"] = "Nerea";
dNerea["genero"] = "M";
dNerea["nota"] = 4;

var dKike = new Dictionary<string, Object>();
dKike["nombre"] = "Kike";
dKike["genero"] = "H";
dKike["nota"] = 6.5M;

var dJuan = new Dictionary<string, Object>();
dJuan["nombre"] = "Juan";
dJuan["genero"] = "H";
dJuan["nota"] = 7.5M;

var notas = new [] {dLuis, dMarta, dMarcos, dAroa, dNerea, dKike, dJuan};

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
        suma += Convert.ToDecimal(notas[i]["nota"]);
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
        if (Convert.ToDecimal(notas[i]["nota"]) >= 5) aprobados++; 
        i++;
    } while (i < notas.Length);

    porcentaje = (100 * aprobados) / notas.Length;

}