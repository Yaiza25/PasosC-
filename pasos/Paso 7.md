using System;
using System.Linq;
using System.Collections.Generic;

namespace proyecto {

    // MAIN

    class Program {

        static void Main(string[] args) {

            // VARIABLES

            var cLuis = new calificacion("Luis", "H", 7.5M);
            var cMarta = new calificacion("Marta", "M", 4);
            var cMarcos = new calificacion("Marcos", "H", 6);
            var cAroa = new calificacion("Aroa", "M", 5);
            var cNerea = new calificacion("Nerea", "M", 4);
            var cKike = new calificacion("Kike", "H", 6.5M);
            var cJuan = new calificacion("Juan", "H", 7.5M);

            var notas = new [] {cLuis, cMarta, cMarcos, cAroa, cNerea, cKike, cJuan};

            var sistema = new sistema(notas);

            // RESULTADO

            Console.WriteLine($"La media es: {sistema.calcularMedia():0.00}");
                    
        }

    }

    //  CONSTRUCTORA

    public class calificacion {

        // VARIABLES

        public string Nombre;
        public string Genero;
        public decimal Nota;

        // CON PARAMETROS

        public calificacion (String nombre, String genero, decimal nota) {
            this.Nombre = nombre;
            this.Genero = genero;
            this.Nota = nota;
        }

        // TO STRING

        public override string ToString() => $"({Nombre}, {Genero}, {Nota}";

    }

    // CONTRUCTORA Y FUNCIONES

    public class sistema {

        // VARIABLES

        calificacion[] Notas;

        // CON PARAMETROS

        public sistema(calificacion[] notas) {
            this.Notas = notas;
        }

        // FUNCIONES

        private decimal calcularSuma(decimal[] datos) {return datos.Sum();}

        public decimal calcularMedia() {

            var notas2 = Notas.Select(n => n.Nota).ToArray();
            return calcularSuma(notas2) / notas2.Length;

        }
        
    }

}