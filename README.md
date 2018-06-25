# Pic
```python
import matplotlib as plt
import pylab

# Used for pertubation
eps = 0.01
# u is the dependent variable here
u = 0.0
# Small perturbation in u
u = u + eps

# r is plotted on the x-axis. Different
# values of r produce very different behaviour.
# In population dynamics r represents the
# combined rate for reproduction and starvation
r = 0.0
dr = 0.005

# Stored the x-axis values in r_list,
# and the y-axis values in u_list
r_list = []
u_list = []

while r < 4.0:
    # Throw away the 1st 200 iterations so that we
    # converge to the equilibria
    for i in range(1,200):
        u = r*u*(1.0 - u)
    for i in range(1,500):
        u = r*u*(1.0 - u)
        r_list.append(r)
        u_list.append(u)
    r = r+dr
    u = u+eps

# Plot the output
plt.pyplot.scatter(r_list,u_list, s=1, facecolor='0.1', lw = 0)
plt.pyplot.axis([1.0,4.0,0,1.0])
plt.pyplot.show()
```

```python
from itertools import repeat
import numpy as np

NUM_RUNS = 1000

def logistic_map(xn, r):
    return r * xn * (1 - xn)

def calculate_fixpoint(x0, r):
    xN = x0
    for _ in repeat(None, NUM_RUNS):
        xN = logistic_map(xN, r)
    return xN

def write_to_file(r, xs):
    np.savetxt('./data/{}.txt'.format(r), xs)

for r in np.arange(0, 4, 0.1):
    xs = np.linspace(0.1, 1, 1000)
    write_to_file(r, calculate_fixpoint(xs, r))
```

```cpp
#include <stdio.h>

// Allocate a pixel array to store generated image
int p[800][800];

int main()
{
	// Algorithm parameters
	int w=640, h=480;
	int m, n, M=10000;
	double r, r_min=3, r_max=4;
	double x, x0=0.5;
	
	// Set every pixel to zero initially
	printf("Blanking canvas\n");
	for (n=0 ; n<h ; ++n) for (m=0 ; m<w ; ++m) p[n][m] = 0;
	
	// Calculate points and add to image.
	// Each new point inrements a single pixel's value.
	// Pixels saturate at 255.
	printf("Calculating and plotting points\n");
	for (n=0 ; n<w ; ++n)
	{
		// Calculate points for current r value
		r = r_min + (n/(double)w)*(r_max - r_min);
		printf("r = %lf\n", r);
		x = x0;
		for (m=0 ; m<M ; ++m)
		{
			x = r * x * (1 - x);
			if (p[(int)(h*x)][n] < 255) p[(int)(h*x)][n]++;
		}
	}
	
	// Write image to PGM file
	printf("Writing PGM file\n");
	FILE *f = fopen("graph.pgm", "w");
	fprintf(f, "P2\n# comment\n%d %d\n255\n", w, h);
	for (n=0 ; n<h ; ++n)
	{
		for (m=0 ; m<w ; ++m) fprintf(f, "%d ", p[n][m]);
		fprintf(f, "\n");
	}
	fclose(f);
	
	return 0;
}
```
