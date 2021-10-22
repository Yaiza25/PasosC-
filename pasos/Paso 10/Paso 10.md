using System;
using System.IO;
using System.Linq;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;

namespace Aplicacion {

    using Services;
    using Negocio;
    using UI.Console;

    class Program {

        static void Main(string[] args) {

            var repositorio = new RepositorioCSV();
            var sistema = new Sistema(repositorio);
            var vista = new Vista();
            var controlador = new Controlador(sistema, vista);

            controlador.Run();

        }

    }

    namespace UI.Console {

        using System;
        using Negocio.Modelos;

        public class Vista {

            public void MostrarObjetos(List<Calificacion> datos) {

                var i = 1;
                
                Console.WriteLine();
                foreach (var item in datos) {Console.WriteLine(i++ + ".- " + item);}

            }

            public void mostrarOpciones() {

                Console.WriteLine("========= Menu =========\n");

                Console.WriteLine("1.- Realizar media\n" + 
                                  "2.- Obtener la mejor nota\n" +
                                  "3.- Informe Suspensos\n" +
                                  "4.- Informe Aprobados H\n" +
                                  "5.- Informe Todos\n");
            }

            public int obtenerOpcion() {

                int opcion = 0;
                bool entradaIncorrecta = true;

                while (entradaIncorrecta) {

                    try {
                        Console.Write("Seleciona una opción: ");
                        opcion = int.Parse(Console.ReadLine());
                        if (opcion == 1 || opcion == 2 || opcion == 3 || opcion == 4 || opcion == 5) entradaIncorrecta = false;
                    } catch (System.Exception) {
                        ;
                    }

                }

                return opcion;

            }

        }

        public class Controlador {

            private Sistema sistema;
            private Vista vista;

            public Controlador(Sistema sistema, Vista vista) {
                this.sistema = sistema;
                this.vista = vista;
            }

            public void Run() {

                Console.Clear();

                Boolean entrada = true;

                while (entrada) {

                    Console.Clear();

                    vista.mostrarOpciones();
                    int opcion = vista.obtenerOpcion();

                    switch(opcion) {

                        case 1:
                            Console.WriteLine($"\n\nLa media de la notas es: {sistema.CalculoDeLaMedia():0.00}");
                        break;

                        case 2:
                            Console.WriteLine($"\n\nLa media de la notas es: {sistema.CalculoDeLaNotaAlta():0.00}");
                        break;

                        case 3:
                            InformeSuspensos();
                        break;

                        case 4:
                            InformeAprobadosH();
                        break;

                        case 5:
                            vista.MostrarObjetos(sistema.LasNotas());
                        break;

                    }

                    Console.WriteLine("\n\nEscriba SI para continuar y NO para finalizar");
                    var confirmacion = Console.ReadLine();

                    while (confirmacion.ToLower() != "no" && confirmacion.ToLower() != "si") {
                        Console.WriteLine("\nEscriba SI para continuar y NO para finalizar");
                        confirmacion = Console.ReadLine();
                    }

                    if (confirmacion.ToLower() == "no") entrada = false;       

                }

            }

            // CASOS DE USO

            private static bool AprobadosH(Calificacion alumno) => alumno.Sexo == "H" && alumno.Nota >= 5;

            private static bool Suspensos(Calificacion alumno) => alumno.Nota < 5;

            public void InformeSuspensos() => InformeGenerico(sistema.LasNotas(), Suspensos);

            public void InformeAprobadosH() => InformeGenerico(sistema.LasNotas(), AprobadosH);

            private void InformeGenerico(List<Calificacion> lista, Func<Calificacion, bool> esValido) {
                
                var alumSeleccionados = new List<Calificacion>();
                
                foreach (Calificacion alumno in lista) {if(esValido.Invoke(alumno)) alumSeleccionados.Add(alumno);};
                vista.MostrarObjetos(alumSeleccionados);

            }

        }

    }

    namespace Negocio {

        namespace Modelos {

            public class Calificacion {

                public string Nombre { get; set; }
                public string Sexo { get; set; }
                public decimal Nota { get; set; }
                public Calificacion(string nombre, string sexo, decimal nota) {
                    Nombre = nombre;
                    Sexo = sexo;
                    Nota = nota;
                }

                public override string ToString() => $"({Nombre}, {Sexo}, {Nota})";

                internal static Calificacion convertirObjeto(string frase) {

                    var columnas = frase.Split(',');
                    return new Calificacion(nombre: columnas[0], sexo: columnas[1], nota: decimal.Parse(columnas[2].Replace('.', ',')));

                }
            }
        }
        public class Sistema {

            IRepositorio Repositorio;

            public Sistema(IRepositorio repositorio) {
                Repositorio = repositorio;
                Repositorio.Inicializar();
            }

            // FUNCION - Calcular la media

            private decimal CalculoDeLaSuma(decimal[] datos) => datos.Sum();
            public decimal CalculoDeLaMedia() {
                var alumnos = Repositorio.CargarCalificaciones();
                var notas = alumnos.Select(calificacion => calificacion.Nota).ToArray();
                return CalculoDeLaSuma(notas) / notas.Length;
            }

            // FUNCION - Devolver la nota mas alta del Array

            public decimal CalculoDeLaNotaAlta() => Repositorio.CargarCalificaciones().Max(alumnos => alumnos.Nota);

            // FUNCION - Cargar la lista de alumnos

            public List<Modelos.Calificacion> LasNotas() => Repositorio.CargarCalificaciones();

        }

    }

    namespace Services {

        using Negocio.Modelos;

        public interface IRepositorio {
            void Inicializar();
            List<Calificacion> CargarCalificaciones();
        }

        public class RepositorioCSV : IRepositorio {

            string datafile;

            void IRepositorio.Inicializar() {
                this.datafile = "notas.csv";
            }

            List<Calificacion> IRepositorio.CargarCalificaciones() => File.ReadAllLines(datafile).Skip(1).Where(linea => linea.Length > 0).Select(Calificacion.convertirObjeto).ToList();

        }

    }

}