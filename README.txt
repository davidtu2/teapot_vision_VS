Name: 			David Tu
Email: 			david.tu2@csu.fullerton.edu
Date: 			11/21/17
Class: 			CPSC 486-02
Assignment: 	teapot-vision
Description: 	This program tests for the visibility of objects. If they are contained within the view 				frustrum, they will show, otherwise they will not.

In general, the program will perform the following steps:
	
	First, it will create an instance of TeapotVisionApp, app, within the main(), which will then create required variables. Of them, the most notible are: 

	mainCamera 			1st person camera
	bevCamera 			Bird's eye view camera
	currentCamera		The current camera the user is viewing through
	shaderProgram		An instance of GLSLProgram
	light0 and light1 	Our light sources
	teapots[20]			20 pointers of UtahTeapot class instances

	Since the TeapotVisionApp has inherited from GLFWApp, the first thing it does is execute the begin() function. By doing so, it initializes many things, including initTeapots(), initCamera() and initLights().

	InitTeapots() will set all the teapot pointers to instances of randomly positioned and randomly colored UtahTeapots. InitCamera() will create the instances of Camera for mainCamera and bevCamera. By the end of that function, it also sets the currentCamera to a reference of the mainCamera. Similarily, initLights() will create the instances of SpinningLight for light0 and light1. Once those initializations are done, the program then proceeds with the initialization of the vertex shader and fragment shader, attching and linking them with the shaderProgram, and finally activating the shaderProgram. This is the last major step for begin().

	After begin(), the program will proceed with render() and will run this function continuously until user feedback indicates that it should end with end(). In general, render() will update the lights, teapots, and the mainCamera, draws them out, and then checks for user feedback every frame.

	In order to update the lights, one of the main things it needs to do is to update the positions of the lights. This is done twice actually - Once for shading and another for drawing. For shading, it just needs to know positions of the lights relative to the camera so it can calculate the light effects correctly. This is done by multiplying the current light positions with the currentCamera's view matrix, lookAtMatrix. To draw the lights, the program needs to know the current modelViewMatrix so that it can draw them in their correct positions on the screen. This can be obtained with a translation of the lookAtMatrix with the lights' position. The drawing process of the teapots and the mainCamera is the same way as this.

	In addition to drawing, the teapots go also through additional logic based on visibility. The function checkVisibility() determines the visibility of all the teapots so that they would be rendered in a particular way. For instance, any visible teapots would be rendered with their own colors, otherwise they would just not show through in the mainCamera. In the bevCamera, you can also observe this, but instead of seeing the teapots in their own colors, you'd see them red if they are in the main camera's view, otherwise they would be white.

	As mentiioned, checkVisibility() is the function that determines the teapots' visibility. It does this by checking if the xyz components of a clip point is within negative w and positive w of the same clip point. The reason why w is used is because it determines the bounds of the image seen on the screen. In order to perform this check, the given point needs to be transformed from model space all the way to clip space first, which can be done by multiplying it with the view and the projection matricies, respectively. This was done for every teapot position so that their visibility was determined.
