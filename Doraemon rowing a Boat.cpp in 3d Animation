#include <GL/glut.h>
#include <cmath>
#include<math.h>
#include <stdlib.h>
#include<stdio.h>

float rotationAngle = 0.0f;  // Initial rotation angle
float rotationSpeed = 1.0f;  // Speed of oscillation (change this to control the pendulum's speed)
float maxAngle = 10.0f;
float cameraAngle = 0.0f;  // This will control the rotation of the camera


float rotationX = 0.0f;  // Rotation along X-axis
float rotationY = 0.0f;  // Rotation along Y-axis
float rotationZ = 0.0f;
static int slices = 16;
static int stacks = 16;


// Function to draw an ellipsoid with a gradient effect
void drawEllipsoid(float centerX, float centerY, float centerZ, float radiusX, float radiusY, float radiusZ) {
    int slices = 50;  // Number of subdivisions around the z-axis
    int stacks = 50;  // Number of subdivisions along the z-axis

    for (int i = 0; i < stacks; ++i) {
        float theta1 = i * M_PI / stacks;
        float theta2 = (i + 1) * M_PI / stacks;

        glBegin(GL_QUAD_STRIP);
        for (int j = 0; j <= slices; ++j) {
            float phi = j * 2 * M_PI / slices;

            // Vertices for current stack (theta1)
            float x1 = centerX + radiusX * cos(phi) * sin(theta1);
            float y1 = centerY + radiusY * cos(theta1);  // This remains constant (up-and-down axis)
            float z1 = centerZ + radiusZ * sin(phi) * sin(theta1);

            // Vertices for next stack (theta2)
            float x2 = centerX + radiusX * cos(phi) * sin(theta2);
            float y2 = centerY + radiusY * cos(theta2);  // This remains constant (up-and-down axis)
            float z2 = centerZ + radiusZ * sin(phi) * sin(theta2);

            // Interpolate colors based on the stack position
            //float colorFactor1 = (float)i / stacks;  // Color interpolation factor for first vertex
            //float colorFactor2 = (float)(i + 1) / stacks;  // Color interpolation factor for second vertex

            // Set color for the first vertex
            //glColor3f(colorFactor1, 0.0f, 1.0f - colorFactor1);  // Gradient from blue to red
            glColor3f(0.0,0.5,1.0);
            glVertex3f(x1, y1, z1);

            // Set color for the second vertex
            //glColor3f(colorFactor2, 0.0f, 1.0f - colorFactor2);  // Gradient from blue to red
            glColor3f(0.0,0.5,1.0);
            glVertex3f(x2, y2, z2);
        }
        glEnd();
    }
}

//cylinder
void drawCylinder(float radius, float height, int slices, float centerX, float centerY, float centerZ) {
    // Move the cylinder to the specified center
    glPushMatrix();
    glTranslatef(centerX, centerY, centerZ);

    glBegin(GL_QUAD_STRIP);
    for (int i = 0; i <= slices; ++i) {
        float angle = 2.0f * M_PI * i / slices;
        float x = cos(angle) * radius;
        float z = sin(angle) * radius;

        glVertex3f(x, 0.0f, z);          // Bottom circle
        glVertex3f(x, height, z);        // Top circle
    }
    glEnd();

    // Draw bottom and top circles
    glBegin(GL_TRIANGLE_FAN);
    glVertex3f(0.0f, 0.0f, 0.0f);  // Center of bottom circle
    for (int i = 0; i <= slices; ++i) {
        float angle = 2.0f * M_PI * i / slices;
        float x = cos(angle) * radius;
        float z = sin(angle) * radius;
        glVertex3f(x, 0.0f, z);
    }
    glEnd();

    glBegin(GL_TRIANGLE_FAN);
    glVertex3f(0.0f, height, 0.0f);  // Center of top circle
    for (int i = 0; i <= slices; ++i) {
        float angle = 2.0f * M_PI * i / slices;
        float x = cos(angle) * radius;
        float z = sin(angle) * radius;
        glVertex3f(x, height, z);
    }
    glEnd();

    glPopMatrix();  // Restore the previous matrix
}

void drawHalfEllipsoid(float radiusX, float radiusY, float radiusZ) {
    int slices = 800;  // Increased number of slices for finer granularity
    int stacks = 50;   // Increased number of stacks for more smoothness

    // Starting color values for gradient
    float r = 0.4f;  // Starting red component
    float g = 0.2f;  // Starting green component
    float b = 0.05f; // Starting blue component

    float colorDecrement = 0.02f;  // Reduced color decrement for smoother transition

    for (int i = stacks / 2 - 1; i >= 0; --i) {  // Start from the topmost stack
        float theta1 = i * M_PI / stacks;       // Current stack angle
        float theta2 = (i + 1) * M_PI / stacks; // Next stack angle

        glBegin(GL_QUAD_STRIP);
        for (int j = 0; j <= slices; ++j) {
            float phi = j * 2 * M_PI / slices;

            // Vertices for current stack (theta1) and next stack (theta2)
            float x1 = radiusX * cos(phi) * sin(theta1);
            float y1 = radiusY * sin(phi) * sin(theta1);
            float z1 = radiusZ * cos(theta1);

            float x2 = radiusX * cos(phi) * sin(theta2);
            float y2 = radiusY * sin(phi) * sin(theta2);
            float z2 = radiusZ * cos(theta2);

            // Set gradient color for current vertices
            glColor3f(r, g, b);
            glVertex3f(x1, y1, z1);

            glColor3f(r, g, b); // Same color for the next vertex of the same loop
            glVertex3f(x2, y2, z2);
        }
        glEnd();

        // Update color values for the next stack (gradually lighten the color)
        r = fmax(0.0f, r - colorDecrement);
        g = fmax(0.0f, g - colorDecrement);
        b = fmax(0.0f, b - colorDecrement);
    }
}

void cuboid(float x, float y, float z, float length, float breadth, float height) {
    // DOWN FACE -- BLACK
    glBegin(GL_QUADS);
    glColor3f(0.0,0.0,0.0);
    glVertex3f(-x - length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z + height / 2);
    glVertex3f(-x - length / 2, y - breadth / 2, z + height / 2);
    glEnd();

    // TOP FACE -- LIGHT BROWN
    glBegin(GL_QUADS);
    glColor3f(0.4f, 0.2f, 0.1f);
    glVertex3f(-x - length / 2, y + breadth / 2, z - height / 2);
    glVertex3f(-x + length / 2, y + breadth / 2, z - height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z - height / 2);
    glVertex3f(-x - length / 2, y - breadth / 2, z - height / 2);
    glEnd();

    // LEFT FACE -- REDDISH BROWN
    glBegin(GL_QUADS);
    glColor3f(0.5f, 0.1f, 0.05f);
    glVertex3f(-x - length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x - length / 2, y + breadth / 2, z - height / 2);
    glVertex3f(-x - length / 2, y - breadth / 2, z - height / 2);
    glVertex3f(-x - length / 2, y - breadth / 2, z + height / 2);
    glEnd();

    // RIGHT FACE -- DARK REDDISH BROWN
    glBegin(GL_QUADS);
    glColor3f(0.5f, 0.1f, 0.05f);
    glVertex3f(-x + length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y + breadth / 2, z - height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z - height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z + height / 2);
    glEnd();

    // Top face (Front Face Color) glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_QUADS);
    glColor3f(0.3f, 0.1f, 0.05f);
    glVertex3f(-x - length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y + breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y + breadth / 2, z - height / 2);
    glVertex3f(-x - length / 2, y + breadth / 2, z - height / 2);
    glEnd();

    // BACK FACE -- Purple
    glBegin(GL_QUADS);
    glColor3f(0.4f, 0.2f, 0.1f);
    glVertex3f(-x - length / 2, y - breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z + height / 2);
    glVertex3f(-x + length / 2, y - breadth / 2, z - height / 2);
    glVertex3f(-x - length / 2, y - breadth / 2, z - height / 2);
    glEnd();
}

void ellipsoid_wave(float centerX, float centerY, float centerZ,
                    float radiusX, float radiusY, float radiusZ) {
    int slices = 800;  // Finer granularity
    int stacks = 50;   // Increased smoothness

    // Starting color values for gradient (Sky blue)
    float r = 0.53f;  // Red component
    float g = 0.81f;  // Green component
    float b = 0.92f;  // Blue component

    float colorDecrement = 0.01f;  // Gradual decrement for smooth transition

    for (int i = 0; i < stacks / 2; ++i) {  // Only bottom half of ellipsoid
        float theta1 = i * M_PI / stacks;       // Current stack angle
        float theta2 = (i + 1) * M_PI / stacks; // Next stack angle

        glBegin(GL_QUAD_STRIP);
        for (int j = 0; j <= slices; ++j) {
            float phi = j * 2 * M_PI / slices;

            // Vertices for current stack (theta1)
            float x1 = centerX + radiusX * cos(phi) * sin(theta1);
            float y1 = centerY + radiusY * sin(phi) * sin(theta1);
            float z1 = centerZ - radiusZ * cos(theta1);  // Invert Z to point downward

            // Vertices for next stack (theta2)
            float x2 = centerX + radiusX * cos(phi) * sin(theta2);
            float y2 = centerY + radiusY * sin(phi) * sin(theta2);
            float z2 = centerZ - radiusZ * cos(theta2);  // Invert Z to point downward

            // Set gradient color for current vertices
            glColor3f(r, g, b);
            glVertex3f(x1, y1, z1);
            glVertex3f(x2, y2, z2);
        }
        glEnd();

        // Update color values for the next stack
        r = fmax(0.0f, r - colorDecrement * 1.2f);
        g = fmax(0.0f, g - colorDecrement * 1.2f);
        b = fmax(0.5f, b - colorDecrement * 0.5f);  // Blue component approaches dark blue
    }
}

void ellipsoid(float centerX, float centerY, float centerZ,
               float radiusX, float radiusY, float radiusZ , float r ,float g ,float b) {
    int slices = 800;  // Finer granularity
    int stacks = 50;   // Increased smoothness

    // Doraemon body blue color (consistent color)
    //r = 0.0f;   // Red component
    //g = 0.5f;   // Green component
    //b = 1.0f;   // Blue component

    float colorDecrement = 0.005f;  // Gradual decrement for a slight gradient

    for (int i = 0; i < stacks; ++i) {
        float theta1 = i * M_PI / stacks;       // Current stack angle
        float theta2 = (i + 1) * M_PI / stacks; // Next stack angle

        glBegin(GL_QUAD_STRIP);
        for (int j = 0; j <= slices; ++j) {
            float phi = j * 2 * M_PI / slices;

            // Vertices for current stack (theta1)
            float x1 = centerX + radiusX * cos(phi) * sin(theta1);
            float y1 = centerY + radiusY * sin(phi) * sin(theta1);
            float z1 = centerZ + radiusZ * cos(theta1);

            // Vertices for next stack (theta2)
            float x2 = centerX + radiusX * cos(phi) * sin(theta2);
            float y2 = centerY + radiusY * sin(phi) * sin(theta2);
            float z2 = centerZ + radiusZ * cos(theta2);

            // Set Doraemon's blue color for current vertices
            glColor3f(r, g, b);
            glVertex3f(x1, y1, z1);
            glVertex3f(x2, y2, z2);
        }
        glEnd();

        // Update color values for the next stack (subtle gradient from light blue to Doraemon blue)
       r = fmax(0.0f, r - colorDecrement);  // Keeping Red low
        g = fmax(0.3f, g - colorDecrement * 0.5f);  // Subtle green decrease
        b = fmax(0.6f, b - colorDecrement);  // Blue stays dominant
    }
}

void ring(float centerX, float centerY, float centerZ, float size) {
    int slices = 800;  // Number of subdivisions around the Z-axis
    float innerRadius = size * 0.8f;  // Smaller inner radius to create ring effect
    float outerRadius = size;  // Outer radius of the ring


    glColor3f(1.0,0.0,0.0);  // Set color for the ring

    for (int i = 0; i < slices; ++i) {
        float theta1 = i * 2 * M_PI / slices;       // Current slice angle
        float theta2 = (i + 1) * 2 * M_PI / slices; // Next slice angle

        glBegin(GL_QUAD_STRIP);
        // Outer ring vertices
        float x1_outer = centerX + outerRadius * cos(theta1);
        float y1_outer = centerY + outerRadius * sin(theta1);
        float x2_outer = centerX + outerRadius * cos(theta2);
        float y2_outer = centerY + outerRadius * sin(theta2);

        // Inner ring vertices
        float x1_inner = centerX + innerRadius * cos(theta1);
        float y1_inner = centerY + innerRadius * sin(theta1);
        float x2_inner = centerX + innerRadius * cos(theta2);
        float y2_inner = centerY + innerRadius * sin(theta2);

        // Drawing quads between inner and outer ring
        glVertex3f(x1_outer, y1_outer, centerZ);
        glVertex3f(x1_inner, y1_inner, centerZ);

        glVertex3f(x2_outer, y2_outer, centerZ);
        glVertex3f(x2_inner, y2_inner, centerZ);

        glEnd();
    }
}
void halfring(float centerX, float centerY, float centerZ) {
    int slices = 400;  // Number of subdivisions
    float radius = 0.40f;  // Default radius value
    float angleRange = M_PI * 1.2f;  // Cover 1.2 * 180 degrees (~216 degrees)

    glColor3f(0.0f, 0.0f, 0.0f);  // Set color for the ring

    glBegin(GL_LINE_STRIP);  // Use LINE_STRIP for single line
    for (int i = 0; i <= slices; ++i) {  // Loop through slices for continuous line
        float theta = i * angleRange / slices;  // Current slice angle
        float x = centerX + radius * cos(theta);
        float y = centerY + radius * sin(theta);

        glVertex3f(x, y, centerZ);  // Plot the points of the single-line ring
    }
    glEnd();
}

void update(int value) {
    // Update rotation angle for pendulum-like motion
    rotationAngle += rotationSpeed;

    if (rotationAngle > maxAngle || rotationAngle < -maxAngle) {
        rotationSpeed = -rotationSpeed;  // Reverse direction when the angle reaches max/min
    }

    // Increment the camera rotation angle (change by 1 degree every frame)
    cameraAngle += 1.0f;
    if (cameraAngle >= 360.0f) {
        cameraAngle = 0.0f;  // Reset after full rotation
    }
    // Apply rotation along the Z-axis for the pendulum
    glLoadIdentity();  // Reset the current transformation matrix
    glRotatef(rotationAngle, 0.0f, 0.0f, 1.0f);  // Rotate around Z-axis

    // Apply the camera rotation around Z-axis
    glRotatef(cameraAngle, .0f, 0.0f, 0.0f);  // Rotate the scene (or camera) along the Z-axis


    glutPostRedisplay();  // Request a redraw of the scene

    // Call the update function again after 25ms (for smooth animation)
    glutTimerFunc(25, update, 0);
}

//boat drawing function
void boat()
{
    //drawWaterWaves();  // Draw water waves around the boat
    drawHalfEllipsoid(2.0f, 1.0f, 1.0f);  // Half ellipsoid with a larger horizontal radius
    cuboid(0.3,0.01,0.01,0.2,1.9,0.1);

    cuboid(-0.4,0.01,0.01,0.2,1.9,0.1);

    cuboid(1.0,0.01,0.01,0.2,1.65,0.1);

    cuboid(-1.1,0.0,0.01,0.2,1.6,0.1);

    cuboid(1.6,0.01,0.01,0.2,1.0,0.1);

    cuboid(-1.6,0.01,0.01,0.2,1.0,0.1);

    //ellipsoid_wave(-0.8,2.0,0.3,0.2,0.2,0.3);



}
// Function to draw Doraemon's body and face
void drawDoraemon() {

    // Draw body - Blue
    ellipsoid(-0.2,0.0,-0.2,0.4,0.4,0.5,0.0f, 0.5f, 1.0f);

    // Draw Head Back-Blue
    ellipsoid(-0.2,0.0,-0.923,0.5,0.5,0.5,0.0f, 0.5f, 1.0f);

    // Draw Head front-white
    ellipsoid(-0.18,0.08,-0.9,0.46,0.46,0.46,1.0f, 1.0f, 1.0f);

    // Draw Head eye-white
    ellipsoid(-0.20,0.5,-1.23,0.1,0.1,0.112,0.827f, 0.827f, 0.827f);
    ellipsoid(-0.0,0.4,-1.24,0.1,0.1,0.112,0.827f, 0.827f, 0.827f);




    // Draw eye-ball
    //ellipsoid(1.0,0.4,-1.24,0.01,0.01,0.01,0.0f, 0.0f, 0.0f);
    glColor3f(0.0f, 0.0f, 0.0f);  // black color for the sphere
    glPushMatrix();
    glTranslatef(-0.2, 0.6,-1.24); // Position sphere
    glScalef(1.0f,1.0f,1.0f);
    glutSolidSphere(0.03, slices, stacks); // Draw sphere ; radius, slices, stacks
    glPopMatrix();

    glColor3f(0.0f, 0.0f, 0.0f);  // black color for the sphere
    glPushMatrix();
    glTranslatef(-0.0, 0.5,-1.24); // Position sphere
    glScalef(1.0f,1.0f,1.0f);
    glutSolidSphere(0.03, slices, stacks); // Draw sphere ; radius, slices, stacks
    glPopMatrix();

    // Draw nose
    glColor3f(1.0f, 0.0f, 0.0f);  // red color for the sphere
    glPushMatrix();
    glTranslatef(-0.1, 0.55,-1.15); // Position sphere
    glutSolidSphere(0.05, slices, stacks); // Draw sphere ; radius, slices, stacks
    glPopMatrix();

    //ring

    ring(-0.2,0.1,-0.5,0.35);
    ring(-0.2,0.1,-0.505,0.35);
    ring(-0.2,0.1,-0.510,0.35);
    ring(-0.2,0.1,-0.515,0.35);
    ring(-0.2,0.1,-0.520,0.35);
    ring(-0.2,0.1,-0.525,0.35);

    // Draw tail
    glColor3f(1.0f, 0.0f, 0.0f);  // red color for the sphere
    glPushMatrix();
    glTranslatef(-0.63,0.1,-0.15); // Position sphere
    glutSolidSphere(0.05, slices, stacks); // Draw sphere ; radius, slices, stacks
    glPopMatrix();

    // Draw white pocket
    ellipsoid(-0.1, 0.1,-0.3,0.3,0.3,0.3,1.0f, 1.0f, 1.0f);
    ellipsoid(0.15, 0.15,-0.25,0.1,0.2,0.1,0.827f, 0.827f, 0.827f);


    // Draw bell
    glColor3f(0.9f, 0.75f, 0.0f);  // Slightly darker yellow
    glPushMatrix();
    glTranslatef(0.13, 0.2,-0.5); // Position sphere
    glutSolidSphere(0.05, slices, stacks); // Draw sphere ; radius, slices, stacks
    glPopMatrix();

    //mouth
    float x = -0.15f;
    float y = 0.165f ;  // Increase the y-coordinate to move upwards
    float z = -0.905f;

    glPushMatrix();  // Save the current transformation matrix
    glTranslatef(x, y, z);
    glRotatef(35.0f, 1.0f, 0.0f, 0.0f);
    glLineWidth(2);
    halfring(0,0,0 );
    glPopMatrix();

    // nose
     x = -0.08f;
     y = 0.14f ;  // Increase the y-coordinate to move upwards
     z = -0.916f;

    glPushMatrix();  // Save the current transformation matrix
    glTranslatef(x, y, z);
    glRotatef(96.0f, 0.0f, 1.0f, 0.0f);
    glLineWidth(2);
    halfring(0,0,0 );
    glPopMatrix();
    //glRotatef(105.0f, 1.0f, 0.0f, 0.0f);
    //halfring(-0.84, -0.8,-2.0,0.15 );
    //halfring(-0.18,0.08,-0.9,0.15);

    //moustach line
    glBegin(GL_LINES);
    glVertex3f(0.01, 0.55,-1.05);
    glVertex3f(0.22, 0.55,-1.2);
    glEnd();
    glBegin(GL_LINES);
    glVertex3f(0.02, 0.55,-1.0);
    glVertex3f(0.3, 0.55,-1.09);
    glEnd();
    glBegin(GL_LINES);
    glVertex3f(0.031, 0.55,-0.95);
    glVertex3f(0.32, 0.55,-1.0);
    glEnd();

    glBegin(GL_LINES);
    glVertex3f(-0.2, 0.55,-1.05);
    glVertex3f(-0.45, 0.55,-1.12);
    glEnd();
    glBegin(GL_LINES);
    glVertex3f(-0.22, 0.55,-0.99);
    glVertex3f(-0.55, 0.55,-1.01);
    glEnd();
    glBegin(GL_LINES);
    glVertex3f(-0.2, 0.55,-0.92);
    glVertex3f(-0.55, 0.55,-0.9);
    glEnd();


    //front hand

    // Set up the camera/viewing angle using gluLookAt
    //GLdouble eyeX = 4.0f, eyeY = 2.0f, eyeZ = 5.0f;  // Camera position
    //GLdouble centerX = 0.0f, centerY = 0.0f, centerZ = 0.0f;  // Look at the origin (0,0,0)
    //GLdouble upX = 0.0f, upY = 1.0f, upZ = 0.0f;  // Up direction (standard vertical)

    //gluLookAt(eyeX, eyeY, eyeZ, centerX, centerY, centerZ, upX, upY, upZ);

    // Translate the ellipsoid so that the point of rotation is at the opposite end of the ellipsoid
    float translationX = -0.2f;  // Translation to move the rotation point to the left end of the ellipsoid
    glTranslatef(translationX, 0.0f, 0.0f);  // Move to the opposite end of the ellipsoid along the x-axis

    // Rotate the ellipsoid by the current rotation angle (pendulum effect) around the Y-axis
    glRotatef(rotationAngle, 0.0f, 1.0f, 0.0f);  // Rotate around the Y-axis

    // Now move the ellipsoid back to its original position (so that it rotates around the opposite end)
    glTranslatef(-translationX, 0.0f, 0.0f);  // Translate back to the original position

    // Draw the ellipsoid
    drawEllipsoid(-0.08,0.48,-0.45, 0.25f, 0.1f, 0.1f);  // Center at (0,0,0) with radii

    // Draw Head front-white
    ellipsoid(0.16,0.48,-0.45,0.08,0.08,0.08,1.0f, 1.0f, 1.0f);


    // translationX = -0.2f;  // Translation to move the rotation point to the left end of the ellipsoid
    glTranslatef(translationX, 0.0f, 0.0f);  // Move to the opposite end of the ellipsoid along the x-axis

    // Rotate the ellipsoid by the current rotation angle (pendulum effect) around the Y-axis
    glRotatef(rotationAngle, 0.0f, 1.0f, 0.0f);  // Rotate around the Y-axis

    // Now move the ellipsoid back to its original position (so that it rotates around the opposite end)
    glTranslatef(-translationX, 0.0f, 0.0f);  // Translate back to the original position

    // Draw the ellipsoid
    drawEllipsoid(-0.08,-0.45,-0.45, 0.25f, 0.1f, 0.1f);  // Center at (0,0,0) with radii

    // Draw Head front-white
    ellipsoid(0.16,-0.45,-0.45,0.08,0.08,0.08,1.0f, 1.0f, 1.0f);

    //rowing
    glRotatef(120.0f, 0.0f, 0.0f, 1.0f);

    glColor3f(0.9f, 0.75f, 0.0f);
    drawCylinder(0.02, 2, 32, -0.5,-0.0,-0.45);
    ellipsoid(2.3,-0.0,-0.45,0.08,0.01,0.08,1.0f, 1.0f, 0.2f);

    glColor3f(0.9f, 0.75f, 0.0f);
    glRotatef(100.0f, 0.0f, 0.0f, 1.0f);
    drawCylinder(0.02, 2, 32, -0.45,-2.25,-0.45);
    ellipsoid(2.1,0.2,-0.45,0.2,0.02,0.1,1.0f, 1.0f, 0.2f);


}

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glLoadIdentity();

    // Disable depth test for background
    glDisable(GL_DEPTH_TEST);

    // Set orthographic projection for 2D background
    glMatrixMode(GL_PROJECTION);
    glPushMatrix();  // Save current projection matrix
    glLoadIdentity();
    gluOrtho2D(-1.0, 1.0, -1.0, 1.0);  // Set 2D orthographic projection

    glMatrixMode(GL_MODELVIEW);
    glPushMatrix();  // Save current modelview matrix
    glLoadIdentity();

    // Draw gradient background as a fullscreen quad
    glBegin(GL_QUADS);

    // Top vertex (dark blue)
    glColor3f(0.0f, 0.0f, 0.5f);  // Dark blue
    glVertex2f(-1.0f, 1.0f);
    glVertex2f(1.0f, 1.0f);

    // Bottom vertex (sky blue)
    glColor3f(0.53f, 0.81f, 0.98f);  // Sky blue
    glVertex2f(1.0f, -1.0f);
    glVertex2f(-1.0f, -1.0f);

    glEnd();

    // Restore previous projection and modelview matrices
    glPopMatrix();
    glMatrixMode(GL_PROJECTION);
    glPopMatrix();
    glMatrixMode(GL_MODELVIEW);

    // Re-enable depth test for 3D objects
    glEnable(GL_DEPTH_TEST);

    // Set 3D perspective for objects
    glLoadIdentity();
    glTranslatef(0.0f, 0.0f, -5.0f);  // Move the object back

    // Apply rotation for the camera view angle
    glRotatef(cameraAngle, 1.0f, 0.0f, 0.0f);  // Rotate around Z-axis by cameraAngle

    // Rotate the object itself as needed
    glRotatef(rotationX, 1.0f, 0.0f, 0.0f);
    glRotatef(rotationY, 0.0f, 1.0f, 0.0f);

    // Draw 3D objects
    boat();
    drawDoraemon();

    glutSwapBuffers();
}

void initOpenGL() {
    glEnable(GL_DEPTH_TEST);  // Enable depth testing for 3D rendering

    // Set up the projection matrix
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0f, 1.0f, 0.1f, 100.0f);  // Set up perspective projection
    glMatrixMode(GL_MODELVIEW);  // Switch back to model-view matrix
}

void reshape(int width, int height) {
    glViewport(0, 0, width, height);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(45.0, (float)width / (float)height, 1.0, 100.0);
    glMatrixMode(GL_MODELVIEW);
}

void keyboard(int key, int x, int y) {
    switch (key) {
        case GLUT_KEY_UP:
            rotationX -= 5.0f;  // Rotate the object up (toward positive X-axis)
            break;
        case GLUT_KEY_DOWN:
            rotationX += 5.0f;  // Rotate the object down (toward negative X-axis)
            break;
        case GLUT_KEY_LEFT:
            rotationY -= 5.0f;  // Rotate the object left (counterclockwise around Y-axis)
            break;
        case GLUT_KEY_RIGHT:
            rotationY += 5.0f;  // Rotate the object right (clockwise around Y-axis)
            break;
    }
    glutPostRedisplay();  // Request to redraw the screen with updated rotation
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
    glutInitWindowSize(800, 700);
    glutInitWindowPosition(10, 10);
    glutCreateWindow("Doraemon rowing a Boat");
     initOpenGL();

    glEnable(GL_DEPTH_TEST);

    // Set background color to light sky blue
    glClearColor(0.53f, 0.81f, 0.98f, 1.0f);  // Light sky blue

    glutDisplayFunc(display);
    glutTimerFunc(10, update, 0);
    glutReshapeFunc(reshape);
    glutSpecialFunc(keyboard);  // Use special function for arrow keys

    glutMainLoop();
    return 0;
}
