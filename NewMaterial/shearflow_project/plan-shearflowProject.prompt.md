# Shearflow Project Plan

## Overview
Create two Jupyter notebooks for a group exercise: one for preprocessing 1D profiles from a user-specified shearflow simulation downloaded from the Well benchmark, and one for training a 3-point finite difference stencil on anomalous diffusion.

## Notebook 1: Data Preprocessing (`shearflow_preprocessing.ipynb`)

### Purpose
Download a single shearflow simulation and preprocess it into 1D spatial profiles as time series for stencil learning.

### User Inputs
- **Reynolds number**: 1e4, 5e4, 1e5, or 5e5
- **Schmidt number**: 0.1, 0.2, 0.5, 1.0, 2.0, 5.0, or 10.0
- **Initial condition index**: 0-39 (one initial condition per case)

### Processing Steps
1. **Download Dataset**: Construct URL from The Well benchmark and download HDF5 file
   - Format: `https://polymathic-ai.org/the_well/datasets/shear_flow/shear_flow_Re{Re}_Sc{Sc}_ic{ic}.hdf5`
   - ~1-2 GB per case

2. **Explore HDF5 Structure**: Print dataset contents and metadata

3. **Extract Tracer Field**: Load tracer/scalar field from HDF5 at all 200 time steps
   - Expected shape: (time=200, y=512, x=256)

4. **Average Perpendicular to Density Gradient**: 
   - Density gradient is in y-direction (vertical)
   - Average across x-direction to obtain 1D spatial profiles s(y,t)
   - Final shape: (time=200, y=512)

5. **Compute Anomalous Diffusivity Metrics**: Estimate decay rate from variance reduction

6. **Visualize 1D Profiles**: 
   - Tracer at different time snapshots
   - Heatmap of spatiotemporal evolution
   - Variance decay over time
   - Mean profile and variability

7. **Save Preprocessed Data**:
   - Tracer numpy array (.npy)
   - Coordinates (.npz): y, t
   - Metadata (.json): Re, Sc, ic, grid info, decay rate

### Output
- `preprocessed_data/tracer_Re{Re}_Sc{Sc}_ic{ic}.npy`
- `preprocessed_data/coordinates_Re{Re}_Sc{Sc}_ic{ic}.npz`
- `preprocessed_data/metadata_Re{Re}_Sc{Sc}_ic{ic}.json`

---

## Notebook 2: Stencil Learning (`shearflow_stencil_learning.ipynb`)

### Purpose
Learn a 3-point finite difference stencil that captures anomalous diffusion dynamics using PyTorch automatic differentiation.

### Theory
Learn stencil coefficients [a, b, c] such that:
$$\frac{\partial s}{\partial t} = a \cdot s_{i-1} + b \cdot s_i + c \cdot s_{i+1}$$

Using implicit Euler time stepping:
$$s^{n+1}_i = s^n_i + \Delta t (a s^{n+1}_{i-1} + b s^{n+1}_i + c s^{n+1}_{i+1})$$

Which forms the linear system:
$$(I - \Delta t D) s^{n+1} = s^n$$

where D is the tridiagonal matrix encoding the stencil.

### Training Steps
1. **Load Preprocessed Data**: Read tracer and coordinates from preprocessing notebook

2. **Data Preparation**: Convert to PyTorch tensors, compute dt and dy

3. **Build Learnable Stencil Matrix**: 
   - 3-point stencil with periodic boundary conditions
   - Stencil coefficients [a, b, c] as learnable parameters

4. **Training Loop**:
   - Initialize with centered difference stencil: [1, -2, 1] / dy²
   - For each epoch:
     - Build differentiation matrix from current coefficients
     - Form implicit system: A = I - dt·D
     - Step through all time steps: solve A·u^{n+1} = u^n
     - Compute MSE loss vs true solution
     - Backpropagate through all time steps
     - Update coefficients with Adam optimizer
   - Run for ~5000 epochs

5. **Evaluate on Training Data**:
   - Roll out learned stencil
   - Compute relative L² error
   - Compare error at different time snapshots

6. **Visualize Results**:
   - Loss convergence curve (semilogy)
   - Learned vs expected stencil coefficients (bar chart)
   - True vs learned solutions at 6 time snapshots
   - Error metrics table

7. **Estimate Diffusion Coefficient**:
   - Extract effective diffusion from learned stencil
   - Compare to theoretical value D = 1/(Re·Sc)
   - Compute relative error

### Outputs
- Learned stencil coefficients
- Training convergence plots
- Solution comparison plots
- Effective diffusion coefficient estimate
- Error metrics

---

## Implementation Details

### Technologies
- **PyTorch**: Automatic differentiation, linear solvers (torch.linalg.solve)
- **NumPy**: Data loading, numerical computation
- **Matplotlib**: Visualization
- **h5py**: HDF5 file I/O
- **requests**: Download from URL

### Key Functions

#### Preprocessing Notebook
```python
def load_hdf5_and_extract_tracer(filepath):
    # Load HDF5 file and extract tracer at all time steps
    
def average_perpendicular_to_gradient(tracer_3d):
    # Average along x-direction to get 1D profiles
    
def compute_variance_decay(tracer_1d):
    # Estimate anomalous diffusivity from variance reduction
```

#### Stencil Learning Notebook
```python
def build_learnable_stencil_periodic(N, coeffs):
    # Build tridiagonal matrix with periodic BCs
    # Returns: D matrix (N, N)
    
def train_stencil(tracer_data, num_epochs=5000, lr=0.01):
    # Main training loop using implicit Euler + PyTorch autodiff
    # Returns: learned coefficients, loss history
```

### Training Configuration
- **Optimizer**: Adam (lr=0.01)
- **Epochs**: 5000 (with checkpoints at 0, 100, 200, ...)
- **Time integration**: Implicit Euler
- **Boundary conditions**: Periodic in y
- **Loss function**: Mean squared error between predicted and true tracer

### Expected Results
For Re=1e4, Sc=1.0:
- Theoretical diffusion: D = 1/(Re·Sc) = 1e-4
- Learned stencil should converge to [~1e-4, ~-2e-4, ~1e-4] (normalized by dy²)
- Relative L² error: < 1e-2 for well-resolved time stepping

---

## Exercises for Students

### Basic
1. Run preprocessing with different Reynolds and Schmidt numbers
2. Observe how diffusion coefficient changes with Re and Sc
3. Train stencils and compare learned D to theoretical 1/(Re·Sc)

### Intermediate
1. Modify training to use 5-point stencils [a, b, c, d, e]
2. Compare 3-point vs 5-point convergence and final error
3. Experiment with different learning rates and optimizers

### Advanced
1. Train on multiple initial conditions simultaneously
2. Study how stencil varies across different parameter regimes
3. Implement spatial or temporal variation in stencil coefficients
4. Investigate whether constant stencil is appropriate for anomalous diffusion

---

## File Structure
```
shearflow_project/
├── shearflow_preprocessing.ipynb          # Data download and preprocessing
├── shearflow_stencil_learning.ipynb       # Stencil learning and training
├── preprocessed_data/                     # Output of preprocessing notebook
│   ├── tracer_Re*.npy
│   ├── coordinates_Re*.npz
│   └── metadata_Re*.json
└── README.md                              # Instructions for students
```

---

## Notes

### Dataset Characteristics
- **Shear flow**: Velocity profile u_x(y) with vertical density gradient
- **Tracer transport**: ∂s/∂t - D·∇²s = -(u·∇)s with Schmidt number Sc
- **Periodic boundary conditions**: In x and y
- **Grid**: 256×512 spatial points, 200 time steps per case
- **Physical parameters vary**: Re ∈ [1e4, 5e5], Sc ∈ [0.1, 10]

### Preprocessing Considerations
- Download files are ~1-2 GB; students should delete after preprocessing
- HDF5 structure varies slightly depending on generator; code should be robust
- Periodic BC verified from dataset documentation

### Stencil Learning Considerations
- Implicit Euler is unconditionally stable but diffusive; compare to RK4 if needed
- PyTorch's torch.linalg.solve is efficient for repeated solves with same LHS
- Autodiff through LU solve is supported and derivatives are accurate
- For very long time series, consider training on subsets to manage memory

