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

            public void MostrarObjetos(List<MascotasYDueños> datos) {

                var i = 1;
                
                Console.WriteLine();
                foreach (var item in datos) {Console.WriteLine(i++ + ".- " + item);}

            }

            public int OpcionDatos() {

                Console.WriteLine("\n¿Qué desea introducir?\n");

                Console.WriteLine("1.- Una mascota nueva\n" + 
                                  "2.- Un dueño nuevo\n" +
                                  "3.- Ambos\n");

                int opcion = 0;
                bool entradaIncorrecta = true;

                while (entradaIncorrecta) {

                    try {
                        Console.Write("Seleciona una opción: ");
                        opcion = int.Parse(Console.ReadLine());
                        if (opcion == 1 || opcion == 2 || opcion == 3) entradaIncorrecta = false;
                    } catch (System.Exception) {
                        ;
                    }

                }

                return opcion;

            }

            public string PedirDatosMascota() {
                Console.WriteLine("\nNombre de la mascota: ");
                var nombreM = Console.ReadLine();

                Console.WriteLine("\nTipo de animal: ");
                var especie = Console.ReadLine();

                Console.WriteLine("\nId: ");
                var id = Console.ReadLine();

                var mascota = nombreM + "," + especie + "," + id;

                return mascota;
            }

            public string PedirDatosDueño() {

                Console.WriteLine("\nId: ");
                var id = Console.ReadLine();

                Console.WriteLine("\nNombre del dueño: ");
                var nombreD = Console.ReadLine();

                Console.WriteLine("\nSexo del dueño: ");
                var sexo = Console.ReadLine();

                var dueño = id + "," + nombreD + "," + sexo;

                return dueño;
            }


            public int OpcionMenu() {

                Console.WriteLine("========= Menu =========\n");

                Console.WriteLine("1.- Lista de todos los dueños con sus mascotas\n" + 
                                  "2.- Lista de las mascotas perros con dueño chicos\n" +
                                  "3.- Crear una mascota al mismo tiempo que asignas su dueño\n");

                int opcion = 0;
                bool entradaIncorrecta = true;

                while (entradaIncorrecta) {

                    try {
                        Console.Write("Seleciona una opción: ");
                        opcion = int.Parse(Console.ReadLine());
                        if (opcion == 1 || opcion == 2 || opcion == 3) entradaIncorrecta = false;
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

                    int opcionMenu = vista.OpcionMenu();

                    switch(opcionMenu) {

                        case 1:
                            DueñoMascota();
                        break;

                        case 2:
                            InformePerros();
                        break;

                        case 3:

                            int opcionDatos = vista.OpcionDatos();

                            switch(opcionDatos) {

                                case 1:
                                    sistema.InformeUsuario("mascotas", vista.PedirDatosMascota());
                                break;

                                case 2:
                                    sistema.InformeUsuario("dueños", vista.PedirDatosDueño());
                                break;

                                case 3:
                                    sistema.InformeUsuario("mascotas", vista.PedirDatosMascota());
                                    sistema.InformeUsuario("dueños", vista.PedirDatosDueño());
                                break;

                            }

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

            public void DueñoMascota() => vista.MostrarObjetos(InformeDueñoMascota(sistema.LosDueños(), sistema.LasMascotas()));

            private List<MascotasYDueños> InformeDueñoMascota(List<Dueños> listaDueños, List<Mascotas> listaMascotas) {
                
                List<MascotasYDueños> seleccionados = new List<MascotasYDueños>();
                
                foreach (Mascotas mascota in listaMascotas) {
                    foreach (Dueños dueño in listaDueños) {
                        if (mascota.Id == dueño.Id) seleccionados.Add(new MascotasYDueños(nombreM: mascota.Nombre, especie: mascota.Especie, id: mascota.Id, nombreD: dueño.Nombre, sexo: dueño.Sexo));
                    };
                };

                return seleccionados;

            }

            public void InformePerros() => InformeGenerico(InformeDueñoMascota(sistema.LosDueños(), sistema.LasMascotas()), EsPerro);

            private static bool EsPerro(MascotasYDueños cliente) => cliente.Especie == "perro" && cliente.Sexo == "H";

            private void InformeGenerico(List<MascotasYDueños> lista, Func<MascotasYDueños, bool> esValido) {
                
                var clienteSeleccionados = new List<MascotasYDueños>();
                
                foreach (MascotasYDueños animal in lista) {
                    if(esValido.Invoke(animal)) clienteSeleccionados.Add(animal);
                };

                vista.MostrarObjetos(clienteSeleccionados);

            }

        }

    }

    namespace Negocio {

        namespace Modelos {

            public class Dueños {

                public int Id { get; set; }
                public string Nombre { get; set; }
                public string Sexo { get; set; }
                public Dueños(int id, string nombre, string sexo) {
                    Id = id;
                    Nombre = nombre;
                    Sexo = sexo;
                }

                public override string ToString() => $"({Id}, {Nombre}, {Sexo})";

                internal static Dueños convertirObjeto(string frase) {

                    var columnas = frase.Split(',');
                    return new Dueños(id: Int32.Parse(columnas[0]), nombre: columnas[1], sexo: columnas[2]);

                }
            }
            
            public class Mascotas {

                public string Nombre { get; set; }
                public string Especie { get; set; }
                public int Id { get; set; }
                public Mascotas(string nombre, string especie, int id) {
                    Nombre = nombre;
                    Especie = especie;
                    Id = id;
                }

                public override string ToString() => $"({Nombre}, {Especie}, {Id})";

                internal static Mascotas convertirObjeto(string frase) {

                    var columnas = frase.Split(',');
                    return new Mascotas(nombre: columnas[0], especie: columnas[1], id: Int32.Parse(columnas[2]));

                }
            }

            public class MascotasYDueños {

                public string NombreM { get; set; }
                public string Especie { get; set; }
                public int Id { get; set; }
                public string NombreD { get; set; }
                public string Sexo { get; set; }

                public MascotasYDueños (string nombreM, string especie, int id, string nombreD, string sexo) {
                    NombreM = nombreM;
                    Especie = especie;
                    Id = id;
                    NombreD = nombreD;
                    Sexo = sexo;
                }

                public override string ToString() => $"({NombreM}, {Especie}, {Id}, {NombreD}, {Sexo})";
            }

        }
        
        public class Sistema {

            IRepositorio Repositorio;

            public Sistema(IRepositorio repositorio) {
                Repositorio = repositorio;
                Repositorio.Inicializar();
            }

            // FUNCION - Cargar la lista de dueños

            public List<Modelos.Dueños> LosDueños() => Repositorio.CargarDueños();

            // FUNCION - Cargar la lista de mascotas

            public List<Modelos.Mascotas> LasMascotas() => Repositorio.CargarMascotas();

            // FUNCION - Insertar informacion

            public void InformeUsuario(string tabla, string linea) => Repositorio.InsertarTexto(tabla, linea);

        }

    }

    namespace Services {

        using Negocio.Modelos;

        public interface IRepositorio {
            void Inicializar();
            void InsertarTexto(string tabla, string linea);
            List<Dueños> CargarDueños();
            List<Mascotas> CargarMascotas();
        }

        public class RepositorioCSV : IRepositorio {

            string dataFileDueños;
            string dataFileMascotas;

            void IRepositorio.Inicializar() {
                this.dataFileDueños = "dueños.csv";
                this.dataFileMascotas = "mascotas.csv";
            }

            List<Dueños> IRepositorio.CargarDueños() => File.ReadAllLines(dataFileDueños).Where(linea => linea.Length > 0).Select(Dueños.convertirObjeto).ToList();
            List<Mascotas> IRepositorio.CargarMascotas() => File.ReadAllLines(dataFileMascotas).Where(linea => linea.Length > 0).Select(Mascotas.convertirObjeto).ToList();

            void IRepositorio.InsertarTexto(string tabla, string linea) {

                var archivo = "";

                if(tabla.Equals("dueños")) archivo = dataFileDueños;
                if(tabla.Equals("mascotas")) archivo = dataFileMascotas;

                try {
                    StreamWriter sw = new StreamWriter(archivo, true);
                    sw.WriteLine(linea);
                    sw.Close();
                } catch(Exception e) {
                    Console.WriteLine("Exception: " + e.Message);
                } 

            }
        }

    }

}