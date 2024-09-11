+++
title = 'SDL2 Example'
date = 2024-09-11T00:49:19-04:00
series = ['Documentation']
series_order = 14
+++

## Setting Up The SDL2 Bindings In Sage

First, before Sage will let us call our SDL2 bindings, we need to provide the hooks to call all the functionality we want.

For SDL2, we want some basic screen drawing functions.

```rs
// Setup the window with the given width and height
extern fun sdl2_init(window_width: Int, window_height: Int);
// Draw a circle at the given x, y position with the given diameter and color
extern fun sdl2_draw_circle(x: Int, y: Int, diameter: Int, r: Int, g: Int, b: Int);
// Clear the screen with the given color
extern fun sdl2_clear(r: Int, g: Int, b: Int);
// Render the screen
extern fun sdl2_render();
// Delay for the given number of milliseconds
extern fun sdl2_delay(ms: Int);
// Quit SDL2 and clean up
extern fun sdl2_cleanup();
```

These functions will be implemented in C and compiled into our Sage program.

## Implementing The SDL2 Bindings In C

Now we need to implement these functions in C. We will use the SDL2 library to do this.

{{< alert "code" >}}
**Filename:** `ffi.h`
{{< /alert >}}
```c
#include <SDL2/SDL.h>
#include <math.h>

static SDL_Window* window = NULL;
static SDL_Renderer* renderer = NULL;

// HELPER FUNCTIONS FOR OUR SAGE BINDINGS

void sdl2_init(int window_width, int window_height) {
    // Initialize SDL2
    if (SDL_Init(SDL_INIT_VIDEO) != 0) {
        SDL_Log("Unable to initialize SDL: %s", SDL_GetError());
        return;
    }

    // Create a window
    window = SDL_CreateWindow("SDL2 Window", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, window_width, window_height, 0);
    if (!window) {
        SDL_Log("Failed to create window: %s", SDL_GetError());
        SDL_Quit();
        return;
    }

    // Create a renderer
    renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
    if (!renderer) {
        SDL_Log("Failed to create renderer: %s", SDL_GetError());
        SDL_DestroyWindow(window);
        SDL_Quit();
        return;
    }

    // Set draw color to black as default
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);
    SDL_RenderPresent(renderer);
}

void sdl2_clear(int r, int g, int b) {
    // Set the background color
    SDL_SetRenderDrawColor(renderer, r, g, b, 255);
    SDL_RenderClear(renderer);
}

void sdl2_draw_rect(int x, int y, int width, int height, int r, int g, int b) {
    // Set the draw color for the rectangle
    SDL_SetRenderDrawColor(renderer, r, g, b, 255);

    printf("Drawing rect: %d %d %d %d %d %d %d\n", x, y, width, height, r, g, b);

    // Create the rectangle
    SDL_Rect rect = { x, y, width, height };
    printf("Drawing rect: %d %d %d %d", rect.x, rect.y, rect.w, rect.h);
    // Draw the rectangle
    SDL_RenderFillRect(renderer, &rect);
}

void sdl2_draw_circle(int x, int y, int diameter, int r, int g, int b) {
    // Set the draw color for the circle
    SDL_SetRenderDrawColor(renderer, r, g, b, 255);

    int radius = diameter / 2;
    int centerX = x + radius;
    int centerY = y + radius;

    // Midpoint circle algorithm for drawing circles
    for (int w = 0; w < diameter; w++) {
        for (int h = 0; h < diameter; h++) {
            int dx = radius - w; // Horizontal distance to the center
            int dy = radius - h; // Vertical distance to the center
            if ((dx * dx + dy * dy) <= (radius * radius)) {
                SDL_RenderDrawPoint(renderer, centerX + dx, centerY + dy);
                SDL_RenderDrawPoint(renderer, centerX - dx, centerY + dy);
                SDL_RenderDrawPoint(renderer, centerX + dx, centerY - dy);
                SDL_RenderDrawPoint(renderer, centerX - dx, centerY - dy);
            }
        }
    }
}

// Clean up and close SDL2
void sdl2_cleanup() {
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
}

void sdl2_render() {
    SDL_RenderPresent(renderer);
    SDL_UpdateWindowSurface(window);
}

void sdl2_pump_events() {
    SDL_Event event;
    while (SDL_PollEvent(&event)) {
        if (event.type == SDL_QUIT) {
            SDL_Quit();
            exit(0);
        }
    }
}
// To be continued...
```

The functions above are straightforward C versions of the functions we want to call in Sage. Our actual Sage bindings will handle retrieving the FFI arguments from the VM, and call these helper functions.

{{< alert "code" >}}
**Filename:** `ffi.h`
{{< /alert >}}
```c
// Continued...

// THE ACTUAL SAGE BINDINGS WHICH WILL BE CALLED FROM THE PROGRAM
void __sdl2_init() {
    // Get our two numbers from the buffer
    cell window_width = ffi_ptr[-1], window_height = ffi_ptr[0];
    ffi_ptr -= 2;

    // Initialize SDL2
    sdl2_init(window_width.i, window_height.i);
}

void __sdl2_clear() {
    cell r = ffi_ptr[-2], g = ffi_ptr[-1], b = ffi_ptr[0];
    ffi_ptr -= 3;

    sdl2_clear(r.i, g.i, b.i);
}

void __sdl2_draw_rect() {
    // Get our six numbers from the buffer
    cell x = ffi_ptr[-6], y = ffi_ptr[-5], width = ffi_ptr[-4], height = ffi_ptr[-3], r = ffi_ptr[-2], g = ffi_ptr[-1], b = ffi_ptr[0];
    ffi_ptr -= 7;
    // Draw the rectangle
    sdl2_draw_rect(x.i, y.i, width.i, height.i, r.i, g.i, b.i);
}

void __sdl2_draw_circle() {
    // Get our six numbers from the buffer
    cell x = ffi_ptr[-5], y = ffi_ptr[-4], diameter = ffi_ptr[-3], r = ffi_ptr[-2], g = ffi_ptr[-1], b = ffi_ptr[0];
    ffi_ptr -= 6;

    // Draw the circle
    sdl2_draw_circle(x.i, y.i, diameter.i, r.i, g.i, b.i);
}

void __sdl2_cleanup() {
    // Clean up and close SDL2
    sdl2_cleanup();
}

void __sdl2_render() {
    // Render the window
    sdl2_render();
}


void __sdl2_delay() {
    // Get our number from the buffer
    cell ms = ffi_ptr[0];
    ffi_ptr -= 1;

    // Delay for the specified number of milliseconds
    for (int64_t i=0; i<ms.i/10; i++) {
        // Do nothing
        SDL_Delay(10);
        sdl2_pump_events();
    }
}
```

## Writing A Small SDL2 Demo In Sage

Now that we have our `ffi.h` file set up, we can call our bindings in a Sage program! Here's a program that draws a planet moving around the screen along with a moon orbiting around it.

<!-- Show the MP4 of the demo at "sdl2-demo.mp4" -->

<video width=100% controls autoplay>
    <source src="../../sdl2-demo.mp4" type="video/mp4">
    Your browser does not support the video tag.  
</video>



{{< alert "code" >}}
**Filename:** `main.sg`
{{< /alert >}}
```rs
// Setup the window with the given width and height
extern fun sdl2_init(window_width: Int, window_height: Int);
// Draw a circle at the given x, y position with the given diameter and color
extern fun sdl2_draw_circle(x: Int, y: Int, diameter: Int, r: Int, g: Int, b: Int);
// Clear the screen with the given color
extern fun sdl2_clear(r: Int, g: Int, b: Int);
// Render the screen
extern fun sdl2_render();
// Delay for the given number of milliseconds
extern fun sdl2_delay(ms: Int);
// Quit SDL2 and clean up
extern fun sdl2_cleanup();

fun sin(mut x: Float): Float {
    x %= 3.14159 * 2;
    let mut result = 0.0;
    let mut term = x;
    let mut i = 1;
    while i < 10 {
        result += term;
        term *= -x * x / (2 * i + 1) / (2 * i);
        i += 1;
    }
    return result;
}

fun cos(x: Float): Float {
    return sin(x + 3.14159 / 2);
}


sdl2_init(800, 600);
for let mut i=0; i<800; i+=1; {
    let planet_radius = 32;
    let planet_x = i;
    let planet_y = (256.0 * sin(planet_x as Float / 32.0)) as Int + 300 + planet_radius / 2;

    sdl2_clear(0, 0, 0);
    sdl2_draw_circle(planet_x, planet_y, planet_radius, 0, 255, 255);

    let moon_radius = 8;
    let moon_orbit_radius = 64.0;
    let moon_x = planet_x + (moon_orbit_radius * cos(i as Float * 0.25)) as Int + moon_radius;
    let moon_y = planet_y + (moon_orbit_radius * sin(i as Float * 0.25)) as Int + moon_radius;

    sdl2_draw_circle(moon_x, moon_y, moon_radius, 255, 255, 0);

    sdl2_render();
    sdl2_delay(10);
}

sdl2_cleanup();
println("Goodbye!");
```
{{< alert "code" >}}
**Output:**<br/>
Goodbye!
{{< /alert >}}