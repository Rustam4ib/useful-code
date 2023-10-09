# Code Bible: my collection of Valuable Snippets

### Randomly generated Rotation matrix
```python
# Generate a random 3x3 rotation matrix for rotation around the X-axis
def random_rotation_x(angle):
    return np.array([[1, 0, 0],
                     [0, np.cos(angle), -np.sin(angle)],
                     [0, np.sin(angle), np.cos(angle)]])


# Generate a random 3x3 rotation matrix for rotation around the Y-axis
def random_rotation_y(angle):
    return np.array([[np.cos(angle), 0, np.sin(angle)],
                     [0, 1, 0],
                     [-np.sin(angle), 0, np.cos(angle)]])


# Generate a random 3x3 rotation matrix for rotation around the Z-axis
def random_rotation_z(angle):
    return np.array([[np.cos(angle), -np.sin(angle), 0],
                     [np.sin(angle), np.cos(angle), 0],
                     [0, 0, 1]])

def random_rotation_matrix():
    # Generate random rotation angles for each axis
    angle_x = np.random.uniform(0, 2 * np.pi)
    angle_y = np.random.uniform(0, 2 * np.pi)
    angle_z = np.random.uniform(0, 2 * np.pi)
    # Generate rotation matrices for each axis
    rotation_x = random_rotation_x(angle_x)
    rotation_y = random_rotation_y(angle_y)
    rotation_z = random_rotation_z(angle_z)

    # Combine the rotation matrices to create a random 3x3 transformation matrix
    random_matrix = np.dot(rotation_z, np.dot(rotation_y, rotation_x))
    return random_matrix
```
##### Visualization of rotation matrix
```python
def rotation_visualizer(rot_matrix):
    """
    Visualize the orientation of a 3x3 rotation matrix.

    Args:
    rot_matrix (numpy.ndarray): 3x3 rotation matrix

    This function visualizes the orientation of the X, Y, and Z axes
    before and after the rotation. It uses matplotlib to create frame arrows representing the axes.

    Returns:
        None

    Example: >>> rotation_visualizer(rot_matrix)
    """

    '''if you want randomly generated rotation matrix, uncomment line below'''
    #random_rot_matrix = random_rotation_matrix()

    # Create a figure and a 3D axis
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')

    # Set axis labels
    ax.set_xlabel('X')
    ax.set_ylabel('Y')
    ax.set_zlabel('Z')

    # Set axis limits
    ax.set_xlim([-1, 1])
    ax.set_ylim([-1, 1])
    ax.set_zlim([-1, 1])

    # Create origin arrows for the frame
    arrow_length = 0.5

    # X-axis arrow
    ax.quiver(0, 0, 0, arrow_length, 0, 0, color='red', label='X-axis')
    ax.text(arrow_length, 0, 0, 'X0', color='red', fontsize=12, va='center', ha='center')

    # Y-axis arrow
    ax.quiver(0, 0, 0, 0, arrow_length, 0, color='green', label='Y-axis')
    ax.text(0, arrow_length, 0, 'Y0', color='green', fontsize=12, va='center', ha='center')

    # Z-axis arrow
    ax.quiver(0, 0, 0, 0, 0, arrow_length, color='blue', label='Z-axis')
    ax.text(0, 0, arrow_length, 'Z0', color='blue', fontsize=12, va='center', ha='center')

    # Create rotated arrows based on the transformation matrix with labels
    arrow_labels = ['X1', 'Y1', 'Z1']
    color_labels = ['red', 'green', 'blue']
    for axis, label, color in zip(np.eye(3), arrow_labels, color_labels):
        rotated_arrow = np.dot(axis, rot_matrix.T)
        ax.quiver(0, 0, 0, rotated_arrow[0], rotated_arrow[1], rotated_arrow[2],
                  color=color, alpha=0.7, linewidth=2, label=f'Rotated {label}-axis')
        ax.text(rotated_arrow[0], rotated_arrow[1], rotated_arrow[2], label, color=color, fontsize=12, va='center',
                ha='center')

    # Set legend
    ax.legend()

    # Show the plot
    plt.show()
```
### Randomly generated point cloud with empty specific circle inside
```python
def is_inside_circle(point, center, radius):
    """Check if a point is inside a given circle."""
    return np.sum((point - center)**2) < radius**2

def generate_points_with_empty_circle(num_points, img_size, circle_center, circle_radius):
    """Generate random points, excluding those inside a specified circle."""
    points = np.random.rand(num_points, 2) * img_size
    return np.array([p for p in points if not is_inside_circle(p, circle_center, circle_radius)])

num_points = 5000
img_size = 120
circle_center = np.array([60, 60])
circle_radius = 30



# Generate point cloud
points = generate_points_with_empty_circle(num_points, img_size, circle_center, circle_radius)
# Visualization
plt.scatter(points[:, 0], points[:, 1], color='blue')
plt.show()
```
