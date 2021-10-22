using System;
using System.Linq;
using System.Collections.Generic;

// VARIABLES

var lugares = new [] {"Tokyo", "London", "Rome", "Donlon", "Kyoto", "Paris"};

var sistema = new Sistema(lugares);
var vista = new Vista();
var controlador = new Controlador(sistema, vista);

// PROCEDIMIENTO

controlador.Run();

// VISTA

class Vista {

    public void MostrarArray(string titulo, string[] datos) {
        Console.WriteLine(titulo + "\n");
        for (int i = 0; i < datos.Length; i++) {Console.WriteLine((i + 1) + ".- " + datos[i]);}
    }

    public void MostrarLista(string titulo, List<string> datos) {

        var i = 1;
        Console.WriteLine("\n" + titulo + "\n");

        foreach (var element in datos) {

            Console.Write(i++ + ".- " + element + "\n");

        }

    }

}

// CONTROLADOR

class Controlador {

    private Sistema Sistema;
    private Vista Vista;

    public Controlador(Sistema sistema, Vista vista) {
        this.Sistema = sistema;
        this.Vista = vista;
    }

    public void Run() {

        Console.Clear();

        // LISTA DE LUGARES - Sin ordenar

        ListaLugares();
        
        // LISTA DE LUGARES - Ordenada

        ListaAnagrama();

    }

    public void ListaLugares() => Vista.MostrarArray("Lista de lugares:", Sistema.LosLugares());

    public void ListaAnagrama() => Vista.MostrarLista("Lista de anagramas: ", Sistema.LosAnagrama());

}

// SISTEMA

class Sistema {

    string[] Lugares;

    public Sistema(string[] lugares) {
        this.Lugares = lugares;
    }

    public string[] LosLugares() => Lugares;

    public List<string> LosAnagrama() {

        List<string> listaAnagrama = new List<string>();
        var lugaresDesordenados = DesordenarArray();
        var guardar = "";

        for(int i = 0; i < Lugares.Length; i++) {

            guardar = "";

            for(int j = 0; j < Lugares.Length; j++ ) {
                if(lugaresDesordenados[i].Equals(lugaresDesordenados[j])) guardar += Lugares[j] + " ";
            }

            if (!listaAnagrama.Contains(guardar)) listaAnagrama.Add(guardar);

        }
        
        return listaAnagrama;

    }

    private string[] DesordenarArray() {
        var lugaresDesordenados = new string[Lugares.Length];

        for (int i = 0; i < lugaresDesordenados.Length; i++) {
            char[] letras = (Lugares[i].ToLower()).ToArray();
            Array.Sort(letras);
            lugaresDesordenados[i] = new string(letras);
        }

        return lugaresDesordenados;
    }
    
}