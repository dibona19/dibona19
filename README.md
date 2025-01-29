# 1. Cargar y mostrar una imagen de un carro en Python con OpenCV
import cv2

imagen = cv2.imread("ruta/a/tu/carro.jpg")
if imagen is None:
    print("Error: No se pudo cargar la imagen.")
else:
    cv2.imshow("Imagen del Carro", imagen)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
    cv2.imwrite("ruta/a/guardar/carro.png", imagen)

# 2. Aplicar efectos a la imagen del carro (escala de grises, bordes)
gris = cv2.cvtColor(imagen, cv2.COLOR_BGR2GRAY)
bordes = cv2.Canny(gris, 100, 200)

cv2.imshow("Carro en Escala de Grises", gris)
cv2.imshow("Bordes del Carro", bordes)
cv2.waitKey(0)
cv2.destroyAllWindows()

# 3. Cargar una imagen de un carro como textura en OpenGL
from OpenGL.GL import *
from OpenGL.GLUT import *
from OpenGL.GLU import *
from PIL import Image

def cargar_textura(ruta):
    imagen = Image.open(ruta)
    imagen = imagen.transpose(Image.FLIP_TOP_BOTTOM)
    datos = imagen.convert("RGBA").tobytes()
    textura_id = glGenTextures(1)
    glBindTexture(GL_TEXTURE_2D, textura_id)
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA, imagen.width, imagen.height, 0, GL_RGBA, GL_UNSIGNED_BYTE, datos)
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR)
    glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR)
    return textura_id

def render():
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
    glLoadIdentity()
    glBindTexture(GL_TEXTURE_2D, textura_id)
    glBegin(GL_QUADS)
    glTexCoord2f(0, 0); glVertex3f(-1, -1, 0)
    glTexCoord2f(1, 0); glVertex3f(1, -1, 0)
    glTexCoord2f(1, 1); glVertex3f(1, 1, 0)
    glTexCoord2f(0, 1); glVertex3f(-1, 1, 0)
    glEnd()
    glutSwapBuffers()

glutInit()
glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH)
glutInitWindowSize(800, 600)
glutCreateWindow(b"Textura de un Carro en OpenGL")
glEnable(GL_TEXTURE_2D)
textura_id = cargar_textura("ruta/a/tu/carro.jpg")
glutDisplayFunc(render)
glutMainLoop()

# 4. Manipular la imagen del carro en Python (recortar, redimensionar)
from PIL import Image

imagen = Image.open("ruta/a/tu/carro.jpg")
recorte = imagen.crop((100, 100, 400, 300))
redimensionada = imagen.resize((300, 200))
recorte.save("ruta/a/guardar/carro_recortado.jpg")
redimensionada.save("ruta/a/guardar/carro_redimensionado.jpg")
recorte.show()
redimensionada.show()
