// Author   : Christian Dunn
// Date     : April 4th, 2016
// Purpose  : To simulate an RLC circuit's electric potential over a given time;
//	      from which a graph will then be displayed showing the curve of decay. 
// Compiler : Visual Studio 2013

#include <iostream>
#include <fstream>
#include "windows.h"
#include <cmath>
using namespace std;

const double EULER = exp(1.0);	// Declare a constant to represent e value

// Structure : Circuit
// Purpose   : Used to allow multiple circuits to be created and to store
//             each circuit's values and variables
struct Circuit
{
	char		graphic     ;
	double          voltage     ;
	double          resistance  ;
	double          capacitance ;
	double          timeConst   ;
	ofstream        circuitFile ;
}; // End Circuit

// ***************************************************
//		Function Prototypes
// ***************************************************
void   Title               ()                       ;
char   getGraphic          ()                       ;
double getResistance       ()                       ;
double getCapacitance      ()                       ;
int    getNumberOfCircuits ()                       ;
double getTime             ()                       ;
double getVoltage          ()                       ;
void   graph               ()                       ;
void   CircuitDisplay      (int)                    ;
void   placeCursor         (HANDLE, int, int)       ;
void   displayPrompts      (HANDLE, char, int, int) ;
// ***************************************************

const double MAX_TIME = 0.100 ; // Declare global constant variable to set max time

// Function   : main
// Purpose    : To output prompts and displays to screen
// Parameters : -> Handle  screen	-> int    numDisplay
//		-> int     num		-> int    initialVoltage
//		-> Circuit *arrCRKT	-> double voltageT
int main()
{
	HANDLE screen = GetStdHandle(STD_OUTPUT_HANDLE);

	Title();

	int      numDisplay     = 1                     ;
	int      num            = getNumberOfCircuits() ;
	int      initialVoltage = 0                     ;
	Circuit *arrCRKT        = new Circuit[num]      ;

	for (int i = 0; i < num; i++)
		{
			CircuitDisplay(numDisplay++);

			arrCRKT[i].resistance  = getResistance()                                  ;
			arrCRKT[i].capacitance = getCapacitance()                                 ;
			arrCRKT[i].graphic     = getGraphic()                                     ;
			arrCRKT[i].timeConst   = (arrCRKT[1].resistance)*(arrCRKT[i].capacitance) ;
			cout << endl;
		} // end for

	initialVoltage = getVoltage() ;

	for (int count = 0; count < num; count++)            
	{
		for (double timestep = 0; timestep <= .1; timestep += .01)      
		{
			double voltageT;
			voltageT = initialVoltage * exp(-timestep / arrCRKT[count].timeConst);   

			int yComp = 35 - (int)(34 * voltageT / initialVoltage);            
			int xComp = (int)(timestep * (72 / .1) + 5);                  
			placeCursor(screen, yComp, xComp);
			cout << arrCRKT[count].graphic;
		} // end inner for
	} // end outer for

	system("pause");
	return 0;
} // end main

// Function   : getNumberOfCircuits
// Purpose    : To prompt user to enter the amount of circuits they'd like to simulate
// Parameters : -> int numberOfCircuits
int getNumberOfCircuits()
{
	int numberOfCircuits;

	cout << "Enter number of simulations (max of 5) : ";
	cin  >> numberOfCircuits;
	cout << endl;

	return numberOfCircuits;
} // end getNumberOfCircuits

// Function   : getResistance
// Purpose    : To prompt user of the given circuit's resistance value
// Parameters : -> double resistance
double getResistance()
{
	double resistance;

	cout << "Enter resistance (in kilo-ohms)      : ";
	cin  >> resistance;

	return resistance;
} // end getResistance

// Function   : getCapacitance
// Purpose    : To prompt user of the given circuit's capacitance value
// Parameters : -> double capacitance
double getCapacitance()
{
	double capacitance;

	cout << "Enter capacitance (in micro-farads)  : ";
	cin  >> capacitance;

	return capacitance;
} // end getCapacitance

// Function   : getVoltage
// Purpose    : To prompt user of the given circuit's voltage value
// Parameters : -> double voltage
double getVoltage()
{
	double voltage;

	cout << "Enter initial voltage for simulation : ";
	cin  >> voltage;

	return voltage;
} // end getVoltage

// Function   : getGraphic
// Purpose    : To prompt user of the given circuit's graphic value
// Parameters : -> char graphic
char getGraphic()
{
	char graphic;

	cout << "Enter graphic (i.e. *, ^, @, etc)    : ";
	cin  >> graphic;

	return graphic;
} // end getGraphic

// **************************************************************
//			Other Functions
// **************************************************************
void placeCursor(HANDLE screen, int row, int col)
{
	COORD position;
		position.Y = row;
		position.X = col;

	SetConsoleCursorPosition(screen, position);
}

void displayPrompts(HANDLE screen, char graphic, int y, int x)
{
	placeCursor(screen, y, x);
	cout << graphic << endl;
}

void CircuitDisplay(int i)
{
	cout << "Circuit " << i << endl;
	cout << "_______________\n\n";
}

void Title()
{
	cout << "_______________________\n";
	cout << " RC Circuit Simulator! \n"; 
	cout << "_______________________\n\n";
}

void graph()
{

}
// **************************************************************