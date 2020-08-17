# Task-one
from PySide2.QtWidgets import QApplication, QGridLayout, QLineEdit, QMainWindow, QWidget, QPushButton, QVBoxLayout
from PySide2.QtCore import Qt
from functools import partial
from PySide2.QtGui import QIcon, QPixmap, QFont
from PySide2 import QtWidgets, QtGui

from pyqtgraph import PlotWidget, plot
import pyqtgraph as pg
import sys  # We need sys so that we can pass argv to QApplication
import os
from numpy import sin, sinc, cos, exp
import numpy as np

ERROR_MSG = 'INVALID INPUT'























class graphplot(QtWidgets.QMainWindow):

    def __init__(self, xmin, xmax, t):
        #super(graphplot, self).__init__(xmin, xmax, t )
        super(graphplot,self).__init__()
        self.graphWidget = pg.PlotWidget()
        self.setCentralWidget(self.graphWidget)
        # xmin = int(input("Enter the min X:"))
        # xmax = int(input("Enter the max X:"))
        A = range(xmin, xmax + 1)
        # t=input("Enter the function of x :")
        #t = ":"
        y = []
        x = xmin
        while (x <= (xmax)):
            y.append(eval(t))

            x += 1

        # print(type(x))
        # print(type(y))

        # plot data: x, y values
        self.graphWidget.plot(A, y)
        self.show()
        #self.plot(A, y)
        # print(y)


# Create a subclass of QMainWindow to setup the GUI
class PyCalcUi(QMainWindow):
    """PyCalc's View (GUI)."""

    def __init__(self):
        """View initializer."""
        super().__init__()
        # Set some main window's properties
        self.setWindowTitle('PyCalc')
        self.setFixedSize(235, 235)
        self.generalLayout = QVBoxLayout()

        # Set the central widget
        self._centralWidget = QWidget(self)
        self.setCentralWidget(self._centralWidget)
        self._centralWidget.setLayout(self.generalLayout)
        # Create the display and the buttons
        self._createDisplay()
        self._createButtons()
        self.setIcon()
        # self.evaluateexpression()
        #self.pretend_something_happened()

        # Snip

    #def pretend_something_happened(self):
        # User Did something
        #self.line_edit.setText("User Entered Something")

    def setIcon(self):
        appIcon = QIcon("index.png")
        self.setWindowIcon(appIcon)

    def _createDisplay(self):
        """Create the display."""
        # Create the display widget
        self.display = QLineEdit()
        # Set some display's properties
        self.display.setFixedHeight(35)
        self.display.setAlignment(Qt.AlignRight)
        self.display.setReadOnly(True)
        # Add the display to the general layout
        self.generalLayout.addWidget(self.display)



    def _createButtons(self):
        """Create the buttons."""
        self.buttons = {}
        buttonsLayout = QGridLayout()
        # Button text | position on the QGridLayout

        buttons = {'7': (0, 0),
                   '8': (0, 1),
                   '9': (0, 2),
                   '/': (0, 3),
                   'C': (0, 4),
                   '4': (1, 0),
                   '5': (1, 1),
                   '6': (1, 2),
                   '*': (1, 3),
                   '(': (1, 4),
                   '1': (2, 0),
                   '2': (2, 1),
                   '3': (2, 2),
                   '-': (2, 3),
                   ')': (2, 4),
                   '0': (3, 0),
                   # '00': (3, 1),
                   '.': (3, 2),
                   '+': (3, 3),
                   '=': (3, 4),
                   '^': (4, 4),
                   'plot': (4, 3),
                   'x': (4, 2),
                   ',': (4, 1),
                   "'": (3, 1),
                   "xmin": (5, 2),
                   "xmax": (5, 0),
                   "func":(5,1)

                   }

        # Create the buttons and add them to the grid layout
        for btnText, pos in buttons.items():
            self.buttons[btnText] = QPushButton(btnText)
            self.buttons[btnText].setFixedSize(40, 40)
            buttonsLayout.addWidget(self.buttons[btnText], pos[0], pos[1])
        # Add buttonsLayout to the general layout
        self.generalLayout.addLayout(buttonsLayout)

        # Snip

    def setDisplayText(self, text):
        """Set display's text."""
        self.display.setText(text)
        self.display.setFocus()

    def displayText(self):
        """Get display's text."""
        return self.display.text()

    def clearDisplay(self):
        """Clear the display."""
        self.setDisplayText('')

    # Create a Controller class to connect the GUI and the model
class PyCalcCtrl:
        """PyCalc Controller class."""

        def __init__(self, model, view):
            """Controller initializer."""
            self._evaluate = model
            self._view = view

            # Connect signals and slots
            self._connectSignals()
            # self.plot()

        def _calculateResult(self):
            """Evaluate expressions."""
            result = self._evaluate(expression=self._view.displayText())
            self._view.setDisplayText(result)

        def _buildExpression(self, sub_exp):
            """Build expression."""
            if self._view.displayText() == ERROR_MSG:
                self._view.clearDisplay()

            expression = self._view.displayText() + sub_exp
            self._view.setDisplayText(expression)





        def _connectSignals(self):
            """Connect signals and slots."""
            for btnText, btn in self._view.buttons.items():
                if btnText not in {'=', 'C', 'plot', 'xmin', 'xmax', 'func'}:
                    btn.clicked.connect(partial(self._buildExpression, btnText))

            self._view.buttons['='].clicked.connect(self._calculateResult)
            self._view.display.returnPressed.connect(self._calculateResult)
            self._view.buttons['C'].clicked.connect(self._view.clearDisplay)
            # self._view.buttons['plot'].clicked.connect(self.plot)
            # self._view.buttons['plot'].clicked.connect(lambda:self.ali())
            #i = graphplot(5, 10, 'x**2')
            self._view.buttons['plot'].clicked.connect(lambda: ploto(-10, 10, 'x**2'))
            #self._view.buttons['xmin'].clicked.connect(self.line_value)
            #self._view.buttons['xmin'].clicked.connect(self.line_value)
            #self._view.buttons['xmin'].clicked.connect(self.line_value)

def ploto(xmin, xmax, t):
    i = graphplot(xmin, xmax, t)
    i.plot()




# Create a Model to handle the  operation
def evaluateExpression(expression):
    """Evaluate an expression."""

    try:
        result = str(eval(expression, {}, {}))
    except Exception:
        result = ERROR_MSG

    return result


# Client code
def main():
    """Main function."""
    # Create an instance of QApplication
    pycalc = QApplication(sys.argv)
    # Show the calculator's GUI
    view = PyCalcUi()
    view.show()

    # Create instances of the model and the controller
    model = evaluateExpression
    PyCalcCtrl(model=model, view=view)
    # Execute calculator's main loop
    # Execute the calculator's main loop
    sys.exit(pycalc.exec_())


if __name__ == '__main__':
    main()
