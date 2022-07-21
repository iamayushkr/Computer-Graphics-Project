#include<vector>
#include<string>
#include<windows.h>
#include<chrono>
#include<GL/glut.h>
#define WH 700
#define WW 1000
using namespace std;
using namespace std::chrono;

void draw(int x, int y);

int k = 0;
int n = 200;
int stime = 5;
vector<bool> sorted;
vector<float> v;

void bitmap_output(int xx, int yy, char* string, void* font)
{
    int len, i;

    glRasterPos2f(xx, yy);

    len = (int)strlen(string);


    for (i = 0; i < len; i++) {
        glutBitmapCharacter(font, string[i]);
    }
}

void front()
{
    glClear(GL_COLOR_BUFFER_BIT);

    glColor3f(0.949, 0.525, 0.231);
    bitmap_output(332, 600, (char*)"SJB INSTITUE OF TECHNOLOGY", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(140, 551, (char*)"BGS Health & Education City, Dr Vishnuvardhan Road, Kengeri, Bengaluru -560060", GLUT_BITMAP_HELVETICA_18);

    glColor3f(0.204, 0.518, 0.945);
    bitmap_output(170, 505, (char*)"DEPARTMENT OF COMPUTER SCIENCE & ENGINEERING", GLUT_BITMAP_TIMES_ROMAN_24);

    glColor3f(0.98, 0.918, 0.282);
    bitmap_output(404, 456, (char*)"A Mini Project On:", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(290, 407, (char*)"SORTING ALGORITHMS VISUALIZER", GLUT_BITMAP_TIMES_ROMAN_24);

    glColor3f(0.933, 0.933, 0.933);
    bitmap_output(170, 358, (char*)"PROJECT BY:", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(170, 309, (char*)"Ayush Kumar", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(170, 287, (char*)"1JB19CS030", GLUT_BITMAP_HELVETICA_12);
    bitmap_output(170, 217, (char*)"Ayush Bansal", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(170, 195, (char*)"1JB19CS029", GLUT_BITMAP_HELVETICA_12);

    bitmap_output(676, 358, (char*)"GUIDED BY:", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(676, 309, (char*)"Dr. Bindiya M K", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(676, 287, (char*)"Associate Professor, Dept of CSE, SJBIT", GLUT_BITMAP_HELVETICA_12);
    bitmap_output(676, 217, (char*)"Mrs Prakruthi M K", GLUT_BITMAP_TIMES_ROMAN_24);
    bitmap_output(676, 195, (char*)"Assistant Professor, Dept of CSE, SJBIT", GLUT_BITMAP_HELVETICA_12);

    glColor3f(1.0, 0.0, 0.0);
    bitmap_output(392, 125, (char*)"Press Enter to continue!", GLUT_BITMAP_HELVETICA_18);
    glEnd();
    glFlush();
}

void myinit() {
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(0, WW, 0, WH);
    glMatrixMode(GL_MODELVIEW);
}

void displayText(string s, int x, int y) {
    glRasterPos2f(x, y);
    for (int i = 0; i < s.length(); i++)
        glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_24, s[i]);
}

void displayTime(int diff, string algo) {
    glColor3f(1, 1, 1);
    string s = "Time taken for " + algo + " = " + to_string(diff) + " milliseconds";
    displayText(s, 5, 0.85 * WH);
    glFlush();
}

void displayStat() {
    glColor3f(1, 1, 1);
    string s = "Elements = " + to_string(n);
    displayText(s, 5, 0.95 * WH);
    s = "Time = " + to_string(stime) + " milliseconds/operation";
    displayText(s, 5, 0.9 * WH);
}

void draw(int x, int y) {
    glClear(GL_COLOR_BUFFER_BIT);
    float size = (WW - 2 * (n + 1.0)) / n;
    for (int i = 0; i < n; i++) {
        if (i == x || i == y)
            glColor3f(0, 0, 1);
        else if (sorted[i])
            glColor3f(0, 1, 0);
        else
            glColor3f(1, 0, 0);
        glBegin(GL_POLYGON);
        glVertex2f(2 + i * (2 + size), 0);
        glVertex2f(2 + i * (2 + size), v[i]);
        glVertex2f(2 + i * (2 + size) + size, v[i]);
        glVertex2f(2 + i * (2 + size) + size, 0);
        glEnd();
    }
    displayStat();
    glFlush();
}

void generate() {
    sorted.clear();
    v.clear();
    srand(time(0));
    for (int i = 0; i < n; i++) {
        v.push_back(((float)rand() / RAND_MAX) * WH * 0.8);
        sorted.push_back(false);
    }
    draw(-1, -1);
}

void swap(float* a, float* b) {
    float t = *a;
    *a = *b;
    *b = t;
}

void bubbleSort() {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            draw(j, j + 1);
            if (v[j] > v[j + 1])
                swap(&v[j], &v[j + 1]);
            Sleep(stime);
            draw(j, j + 1);
        }
        sorted[n - i - 1] = true;
        draw(-1, -1);
    }
}

void merge(int l, int m, int r)
{
    int n1 = m - l + 1;
    int n2 = r - m;
    vector<float> L(n1), R(n2);

    for (int i = 0; i < n1; i++)
        L[i] = v[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = v[m + 1 + j];

    int i = 0, j = 0, k = l;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j])
            v[k] = L[i++];
        else
            v[k] = R[j++];
        sorted[k++] = true;
        Sleep(stime);
        draw(-1, -1);
    }
    while (i < n1) {
        v[k] = L[i++];
        sorted[k++] = true;
        Sleep(stime);
        draw(-1, -1);
    }

    while (j < n2) {
        v[k] = R[j++];
        sorted[k++] = true;
        Sleep(stime);
        draw(-1, -1);
    }
}

void mergeSort(int l, int r) {
    if (l >= r) {
        return;
    }
    int m = l + (r - l) / 2;
    mergeSort(l, m);
    mergeSort(m + 1, r);
    merge(l, m, r);
}

int partition(int low, int high) {
    int pivot = v[high];
    int i = (low - 1);

    for (int j = low; j <= high - 1; j++) {
        draw(i, j);
        if (v[j] < pivot) {
            i++;
            swap(&v[i], &v[j]);
        }
        Sleep(stime);
        draw(i, j);
    }
    draw(i + 1, high);
    swap(&v[i + 1], &v[high]);
    Sleep(stime);
    draw(i + 1, high);
    return (i + 1);
}

void quickSort(int low, int high) {
    if (low < high) {
        int pi = partition(low, high);
        sorted[pi] = true;
        Sleep(stime);
        draw(-1, -1);
        quickSort(low, pi - 1);
        quickSort(pi + 1, high);
    }
    else if (low == high) {
        sorted[low] = true;
        draw(-1, -1);
    }
}

void display() {
    glClearColor(0, 0, 0, 1);
    glClear(GL_COLOR_BUFFER_BIT);
    if (k == 0)
        front();
    else
        generate();
}

void clear() {
    glClear(GL_COLOR_BUFFER_BIT);
    glBegin(0);
    glEnd();
    glFlush();
}

void menufunc(int id) {
    switch (id) {
    case 11:  n = 10; generate(); break;
    case 12:  n = 20; generate(); break;
    case 13:  n = 50; generate(); break;
    case 14:  n = 100; generate(); break;
    case 15:  n = 200; generate(); break;
    case 21: {
        auto start = system_clock::now();
        bubbleSort();
        auto stop = system_clock::now();
        auto diff = duration_cast<milliseconds>(stop - start);
        displayTime(diff.count(), "BubbleSort");
    }break;
    case 22: {
        auto start = system_clock::now();
        mergeSort(0, n - 1);
        auto stop = system_clock::now();
        auto diff = duration_cast<milliseconds>(stop - start);
        displayTime(diff.count(), "MergeSort");
    }break;
    case 23: {
        auto start = system_clock::now();
        quickSort(0, n - 1);
        auto stop = system_clock::now();
        auto diff = duration_cast<milliseconds>(stop - start);
        displayTime(diff.count(), "QuickSort");
    }break;
    case 31: stime = 5; draw(-1, -1); break;
    case 32: stime = 20; draw(-1, -1); break;
    case 33: stime = 50; draw(-1, -1); break;
    case 34: stime = 100; draw(-1, -1); break;
    case 35: stime = 500; draw(-1, -1); break;
    case 4:exit(0);
    }
}

void keyboard(unsigned char key, int x, int y)
{
    if (key == 13)
    {
        k = 1;
        glutPostRedisplay();
    }

}

void createMenu() {
    int s1 = glutCreateMenu(menufunc);
    glutAddMenuEntry("10 Numbers", 11);
    glutAddMenuEntry("20 Numbers", 12);
    glutAddMenuEntry("50 Numbers", 13);
    glutAddMenuEntry("100 Numbers", 14);
    glutAddMenuEntry("200 Numbers", 15);

    int s3 = glutCreateMenu(menufunc);
    glutAddMenuEntry("5", 31);
    glutAddMenuEntry("20", 32);
    glutAddMenuEntry("50", 33);
    glutAddMenuEntry("100", 34);
    glutAddMenuEntry("500", 35);

    int s2 = glutCreateMenu(menufunc);
    glutAddMenuEntry("BubbleSort", 21);
    glutAddMenuEntry("MergeSort", 22);
    glutAddMenuEntry("QuickSort", 23);

    glutCreateMenu(menufunc);
    glutAddSubMenu("Randomize", s1);
    glutAddSubMenu("Speed(Time/oper)", s3);
    glutAddSubMenu("Sort", s2);
    glutAddMenuEntry("Exit", 4);
    glutAttachMenu(GLUT_RIGHT_BUTTON);
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_RGB | GLUT_SINGLE);
    glutInitWindowPosition(250, 50);
    glutInitWindowSize(WW, WH);
    glutCreateWindow("CGV Mini Project");
    myinit();
    glutDisplayFunc(display);
    glutKeyboardFunc(keyboard);
    createMenu();
    glutMainLoop();
    glutIdleFunc(display);
    return 0;
}