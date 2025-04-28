//# CG-animation
//computer graphics animation of car 

#include <GL/gl.h>
#include <GL/glut.h>
#include <math.h>

// Global car position
float carPosX = 0.0f;

// Function to draw a circle (for wheels)
void drawCircle(float cx, float cy, float r) {
    glBegin(GL_POLYGON);
    for (int i = 0; i < 360; i++) {
        float angle = i * 3.14159 / 180;
        glVertex2f(cx + cos(angle) * r, cy + sin(angle) * r);
    }
    glEnd();
}

// Draw car body and wheels
void drawCar() {
    // Car body
    glColor3f(1.0, 0.0, 0.0); // Red
    glBegin(GL_POLYGON);
        glVertex2f(carPosX - 0.5f, 0.2f);
        glVertex2f(carPosX - 0.5f, 0.5f);
        glVertex2f(carPosX + 0.5f, 0.5f);
        glVertex2f(carPosX + 0.5f, 0.2f);
    glEnd();

    // Car top
    glColor3f(0.8, 0.0, 0.0); // Dark red
    glBegin(GL_POLYGON);
        glVertex2f(carPosX - 0.3f, 0.5f);
        glVertex2f(carPosX - 0.2f, 0.7f);
        glVertex2f(carPosX + 0.2f, 0.7f);
        glVertex2f(carPosX + 0.3f, 0.5f);
    glEnd();

    // Wheels
    glColor3f(0.0, 0.0, 0.0); // Black
    drawCircle(carPosX - 0.3f, 0.15f, 0.08f);
    drawCircle(carPosX + 0.3f, 0.15f, 0.08f);
}

// Display callback
void display() {
    glClear(GL_COLOR_BUFFER_BIT);

    // Draw road
    glColor3f(0.3, 0.3, 0.3); // Grey
    glBegin(GL_POLYGON);
        glVertex2f(-1.0f, 0.0f);
        glVertex2f(-1.0f, -0.3f);
        glVertex2f(1.0f, -0.3f);
        glVertex2f(1.0f, 0.0f);
    glEnd();

    drawCar();

    glutSwapBuffers();
}
// Keyboard control
void keyboard(unsigned char key, int x, int y) {
    switch (key) {
        case 'a': case 'A':
            carPosX -= 0.05f;
            break;
        case 'd': case 'D':
            carPosX += 0.05f;
            break;
    }
    glutPostRedisplay();
}

// Init OpenGL
void init() {
    glClearColor(0.5f, 0.8f, 1.0f, 1.0f); // Sky blue

    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(-1.0, 1.0, -0.5, 1.0); // 2D view
}

// Main function
int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
    glutInitWindowSize(800, 600);
    glutCreateWindow("Car Moving on a Road");

    init();
    glutDisplayFunc(display);
    glutKeyboardFunc(keyboard);
    glutMainLoop();
    return 0;
}
