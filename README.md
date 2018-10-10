# Tarea-3

/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package infinitygame;

/**
 *
 * @author gilio
 */
public class InfinityGame {

    /**
     * @param args the command line arguments
     */
    
        public static void main(String[] args) {
              Tablero tablero = new Tablero();
              tablero.menu();
        }
    }
    



/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package infinitygame;

/**
 *
 * @author gilio
 */

import java.util.Scanner;
import java.util.InputMismatchException;

public class Tablero {
    
  Scanner teclado = new Scanner(System.in);
  int blanco = 2;
  int portal = 3;
  
  int desafío = 5;
  int f = 6;
  
  public void instrucciones() {
    System.out.println("Instrucciones:");
    System.out.println();
    System.out.println("1.- Cada jugador tendrá 15 puntos de vida(HP) al inicio del juego.");
    System.out.println("2.- Se pueden perder y recuperar los puntos de vida, sin sobrepasar el límite de 15.");
    System.out.println("3.- Cada jugador lanzará al inicio de cada turno 2 dados de 6 caras, avanzando la cantidad de casillas correspondiente a la suma de los 2 dados.");
    System.out.println("4.- Se puede ganar el juego de 2 formas distintas.");
    System.out.println("      a) Llegar a la casilla final en primer lugar, ocupándose los lugares correspondientes según la cantidad de vida con la que quedase cada jugador.");
    System.out.println("      b) Ser el último jugador en pie, es decir, ser el único jugador con puntos de vida, ocupándose los lugares correspondientes según el orden en el cual perdieron todos sus HP.");
    System.out.println("5.- Hay 6 tipos de casillas distintas, que son:");
    System.out.println("      a) Casilla de partida(1) --> Donde todos los jugadores se encuentran inicialmente.");
    System.out.println("                                   Volver a caer en esta casilla luego de iniciada la partida, restaura automáticamente los puntos de vida.");
    System.out.println("      b) Casilla en blanco(2)  --> Al caer en esta casilla se termina el turno actual y se pasa al siguiente."); 
    System.out.println("      c) Casilla portal(3)     --> Caer en esta casilla teletransporta al jugador a otro portal escogido aleatoriamente.");
    System.out.println("      d) Casilla de salud(4)   --> Caer en esta casilla aumente o reduce los puntos de vida en 1 ó 3 de forma aleatoria.");
    System.out.println("      e) Casilla desafío(5)    --> Se escoge un desafío aleatorio, consistente en avanzar/retroceder (de 1 a 5 casillas) o agregar/quitar (de 1 a 4  puntos de vida) a los demás jugadores, todo de forma aleatoria.");
    System.out.println("      f) Casilla final(6)      --> El primer jugador en caer en esta casilla se convierte en el ganador, acabándose el juego en ese momento.");
    System.out.println("6.- Cada jugador ha de ir llevando la cuenta de su posición actual en el tablero.");
    System.out.println();
  }
  
  public void jugadores() {
    System.out.println("¿Cuántas personas van a jugar?");
    int personas = val();
    if (personas > 0) {
      int vida[] = new int[personas];
      int posJugadores[] = new int [personas];
      String jugadores[] = new String[personas];

      for (int i = 0 ; i < personas ; i++) {
        System.out.println("Ingrese el nick del jugador " + (i + 1));
        jugadores[i] =  teclado.nextLine();
        vida[i] = 15;
      }
      
    } else {
      jugadores();
    }
  }   
  
  public void generarTablero() {
    System.out.println("¿De cuántas casillas desea generar el tablero?");
    int casillas = val();
    
    if (20 <= casillas) {
      int tab[] = new int[casillas];
      
      for (int i = 1 ; i < tab.length -1 ; i++ ) {
        tab[0] = 1;
        tab[tab.length - 1] = 6;
        tab[i] = (int) (Math.random() * 4) + 2;
      }    
      
      for (int i : tab) {
        System.out.print(" | " + i + " | ");
      }
      
      System.out.println();
    } else {
      System.out.println("El tablero debe de tener al menos 20 casillas, ingrese el valor correcto.");
      generarTablero();
    }
    
  }
  
  public void desafio(int indiceJugador , int casilla[] , int posJugadores[]) {
    int azar = (int) (Math.random() * 2) + 1;
    
    if (azar == 1) {
      int azar2 = (int) (Math.random() * 2) + 1;
      
      if (azar2 == 1) {
        posJugadores[indiceJugador] = 5;
        int cas1 = (int) (Math.random() * 5) + 1;
        System.out.println("Usted tendrá que avanzar " + cas1 + " casillas");
      }else {
        posJugadores[indiceJugador] = 5;
        int cas2 = (int) (Math.random() * 5) + 1;
        System.out.println("Usted tendrá que retroceder " + cas2 + " casillas");
      } 
      
    } else {
      int azar3 = (int) (Math.random() * 2) + 1;
      
      if (azar3 == 1) {
        posJugadores[indiceJugador] = 5;
        int ph1 = (int) (Math.random() * 4) + 1;
        System.out.println("Usted ha ganado " + ph1 + " puntos de vida");
      } else {
        posJugadores[indiceJugador] = 5;
        int ph2 = (int) (Math.random() * 4) + 1;
        System.out.println("Usted ha perdido " + ph2 + " puntos de vida");
      }   
      
    }
    
  }
  
  public void portal(int indiceJugador , int tablero[] , int posJugador[]) {
    posJugador[indiceJugador] = 3;
   
    System.out.println("Te moverás a otra casilla portal");
    
    
  }
  public void salud(int indiceJugador , int vida[]) {
    int num = (int) (Math.random() * 2) + 1;
    int vidaRandom = (int) (Math.random() * 3 + 1);
    
    if (num == 1){
      vida[indiceJugador] += vidaRandom; 
      System.out.println("Usted ha ganado " + vidaRandom + " de vida");
      
    } else {
      int num2 = (int) (Math.random() * 3) + 1;
      vida[indiceJugador] -= vidaRandom;
      System.out.println("Usted ha perdido " + vidaRandom + "de vida");
    }
  }
  
  public void salirMenu() {
    System.out.println("Felicidades, preferiste ponerte a estudiar en vez de jugar, ahora si pasarás todos tus ramos :)");    
    System.exit(0);
  }
  
  public void jugar() {
      int meditar = 5;
    System.out.println("¿Qué desea hacer?");
    System.out.println("1.- Lanzar dados");
    System.out.println("2.- Meditar");
    int op = val();
    
    if (op == 1) {
    int dado1 = (int)(Math.random() * 6 + 1);
    System.out.println("Dado 1 = " + dado1);
    int dado2 = (int)(Math.random() * 6 + 1);
    System.out.println("Dado 2 = " + dado2);
    int dados = dado1 + dado2;
    System.out.println("La suma de los dados es " + dados);
    System.out.println();
  } else {
       System.out.println("Puedes moverte 3 casillas en cualquier dirección desde tu posición actual");
        System.out.println("Ha recuperado 1 punto de vida");
       int med = meditar - 1;
       int cont = med;
       if (cont == 0) {
           System.out.println("Agotaste todas tus posibilidades de meditar");
           jugar();
       }
      }
    }
  

  
  public void menu(){ 
    Scanner teclado = new Scanner(System.in);
    System.out.println("Escoja la opción a realizar:");
    System.out.println(); 
    System.out.println("1.- Mostrar instrucciones.");
    System.out.println("2.- Determinar cantidad de jugadores");
    System.out.println("3.- Generar tablero.");
    System.out.println("4.- Jugar");
    System.out.println("5.- Guardar el juego y ponerse a estudiar.");
    
    int opcn = val();
    seleccion(opcn);
  }
    
  public void seleccion (int opcn) {
    
    switch (opcn) {
      case 1: instrucciones();
              menu();
      break;
      
      case 2: jugadores();
              menu();
      break;
        
      case 3: generarTablero();
              menu();
      break;
      
      case 4: jugar();
              menu();
      break;
        
      case 5: salirMenu();
      break;
        
      default: menu();
      break;
    }
    
  }  
    
  public int val () {
    int num = 0;
    boolean error; 
     
    do {
        
      try {
        error = false;
        Scanner teclado = new Scanner(System.in);
        num = teclado.nextInt();
      } catch (InputMismatchException e) {
        System.out.println("El caracter ingresado no es numérico o se encuentra fuera del rango establecido, inténtelo nuevamente.");     
        error = true;
      }
      
    } while (error);
    
    return num;
  }
    
}
