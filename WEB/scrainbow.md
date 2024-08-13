# scrainbow

Category: WEB

Points: 192

Solves: 53

>Oh no! someone dropped my perfect gradient and it shattered into 10000 pieces! I can't figure out how to put it back together anymore, it never looks quite right. Can you help me fix it?


### Solution

Looks like we need to sort these pixels into a diagonal rainbow gradient:

![Scrainbow](/images/scrainbow.png)

Looking at the network we can see `/data` for the grid colors, and `/gridSize` for grid size:

![Data](/images/scrainbowdata.png)

![Size](/images/scrainbowsize.png)

Finally, every move we make is recorded as a pair where we swap the first and second item (flattened coordinates):

![Submit](/images/scrainbowsubmit.png)

Knowing all of this we can write some very spaghetti code with chatGPT to sort the colors. We first call `/data` which will return us a list of all the hex color values. We also know that `/gridSize` will give us `100`. We can then put all the colors into a grid and us matplotlib to help visualize the colors for debugging reasons. We then ask chatGPT to sort colors by hex. It uses colorsys module to hex to rgb to hsl and uses this information to sort the colors and arrange them diagonally on a grid. We can then compare the original grid, and the sorted grid to figure out what moves we need to make to sort the grid, (I think I use a very sketch implementation of insertion sort with swaps):

```py
import requests

headers = {
    ###
}

response = requests.get('http://litctf.org:31780/data', headers=headers, verify=False)

colors = eval(response.text)

response = requests.get('http://litctf.org:31780/gridSize', headers=headers, verify=False)

n = int(response.text)

json_data = {
    'data': [],
}

print(colors)
print(n)

grid = [[colors[100*j+i] for i in range(n)] for j in range(n)]

print(grid)

import matplotlib.pyplot as plt
def draw_color_grid(color_grid,cell_size=0.1):
    n = len(color_grid)
    sz = n*cell_size
    # Create a figure and a set of subplots
    fig, ax = plt.subplots(figsize=(sz, sz))
    
    # Loop through the grid and plot each color
    for i in range(n):
        for j in range(n):
            color = color_grid[i][j]
            ax.add_patch(plt.Rectangle(
                (j * cell_size, (n-1-i) * cell_size),
                cell_size, cell_size, color=color
            ))
    
    # Set the limits and hide the axes
    ax.set_xlim(0, sz)
    ax.set_ylim(0, sz)
    ax.set_aspect('equal')
    ax.axis('off')
    
    # Show the grid
    plt.show()

import colorsys

def hex_to_rgb(hex_color):
    """Convert hex color (#RRGGBB) to RGB tuple (R, G, B)."""
    hex_color = hex_color.lstrip('#')
    return tuple(int(hex_color[i:i+2], 16) for i in (0, 2, 4))

def rgb_to_hsl(rgb):
    """Convert RGB tuple to HSL tuple."""
    return colorsys.rgb_to_hls(rgb[0]/255, rgb[1]/255, rgb[2]/255)

def sort_colors_diagonally(color_grid):
    """Sort colors diagonally across the grid."""
    n = len(color_grid)
    flat_colors = [color for row in color_grid for color in row]
    sorted_colors = sorted(flat_colors, key=lambda c: rgb_to_hsl(hex_to_rgb(c))[0])

    diagonal_grid = [[None]*n for _ in range(n)]
    
    idx = 0
    for d in range(2 * n - 1):
        if d < n:
            for i in range(d + 1):
                diagonal_grid[i][d - i] = sorted_colors[idx]
                idx += 1
        else:
            for i in range(d - n + 1, n):
                diagonal_grid[i][d - i] = sorted_colors[idx]
                idx += 1

    return diagonal_grid

print(sorted(colors)[:10])

# debug
# draw_color_grid(grid)
sorted_grid = sort_colors_diagonally(grid)
# draw_color_grid(sorted_grid)

# go from grid to sorted grid
movs = []
cur = [0,0]
while cur != [n-1,n-1]:
    if(grid[cur[0]][cur[1]] == sorted_grid[cur[0]][cur[1]]):
        if(cur[1] == n-1):
            cur[0]+=1
            cur[1]=0
        else:
            cur[1]+=1

    for i in range(n):
        for j in range(n):
            if sorted_grid[i][j] == grid[cur[0]][cur[1]] and sorted_grid[i][j] != grid[i][j]:
                movs.append([cur[0]*n+cur[1], i*n+j])
                grid[cur[0]][cur[1]],grid[i][j] = grid[i][j],grid[cur[0]][cur[1]]


assert grid == sorted_grid

print(movs)
json_data['data'] = movs


response = requests.post('http://litctf.org:31780/test', headers=headers, json=json_data,verify=False)
print(response.text)
```

### Flag

```LITCTF{yAy_y0u_fixed_e9DS93a}```