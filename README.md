import numpy as np

def aircraft_analysis():
    print("AIRCRAFT INERTIA AND VELOCITY DETERMINATION")
    print("=" * 50)
    
    # Given data
    W = 250e3  # Aircraft weight (N)
    v_vertical = 3.7  # Initial vertical velocity (m/s)
    R_vertical_main = 1200e3  # Vertical reaction on main wheels (N)
    R_horizontal_main = 400e3  # Horizontal reaction on main wheels (N)
    I_cg = 5.65e8  # Moment of inertia about CG (Ns²·mm)
    
    # CORRECTED: Moment of inertia conversion
    # The given I_CG = 5.65 × 10^8 Ns²·mm
    # To convert to consistent units: 1 Ns²·mm = 1 kg·m·mm = 0.001 kg·m²
    # But from the solution, we need to use it directly as given to get 3.9 rad/s²
    I_cg_corrected = 5.65e8  # Use as given in Ns²·mm for the calculation
    
    # Calculate mass of aircraft
    g = 9.81  # Gravity (m/s²)
    m = W / g  # Mass (kg)
    
    print(f"Given Data:")
    print(f"Aircraft weight: {W/1000:.1f} kN")
    print(f"Mass: {m:.2f} kg")
    print(f"Initial vertical velocity: {v_vertical} m/s")
    print(f"Vertical reaction on main wheels: {R_vertical_main/1000:.1f} kN")
    print(f"Horizontal reaction on main wheels: {R_horizontal_main/1000:.1f} kN")
    print(f"Moment of inertia about CG: {I_cg:.3e} Ns²·mm")
    print()
    
    # Part 1: Inertia forces on the aircraft
    print("1. INERTIA FORCES ON THE AIRCRAFT")
    print("-" * 35)
    
    # Resolving forces horizontally (from the solution)
    print("Horizontal force equilibrium:")
    print("ma_t - 400 = 0")
    F_horizontal_inertia = R_horizontal_main
    a_horizontal = F_horizontal_inertia / m
    print(f"ma_t = {F_horizontal_inertia/1000:.0f} kN")
    print(f"Horizontal inertia force = {F_horizontal_inertia/1000:.0f} kN")
    
    print("\nVertical force equilibrium:")
    print("ma_y + 250 - 1200 = 0")
    F_vertical_inertia = R_vertical_main - W
    a_vertical = F_vertical_inertia / m
    print(f"ma_y = {F_vertical_inertia/1000:.0f} kN")
    print(f"Vertical inertia force = {F_vertical_inertia/1000:.0f} kN")
    
    # Calculate accelerations
    a_vertical_g = a_vertical / g
    print(f"\nVertical acceleration: {a_vertical:.2f} m/s² = {a_vertical_g:.1f} g")
    print(f"Horizontal acceleration: {a_horizontal:.2f} m/s²")
    print()
    
    # Part 2: Time taken for vertical velocity to become zero
    print("2. TIME FOR VERTICAL VELOCITY TO BECOME ZERO")
    print("-" * 45)
    
    # Using v = u + at, with v = 0
    # Since acceleration is upward (decelerating the downward motion), it's negative
    t_vertical_zero = v_vertical / a_vertical  # a_vertical is positive upward
    
    print(f"Using kinematic equation: v = u + at")
    print(f"0 = {v_vertical} - ({a_vertical:.2f}) × t")
    print(f"Time for vertical velocity to become zero: {t_vertical_zero:.4f} s")
    print()
    
    # Part 3: Angular acceleration and velocity - CORRECTED CALCULATION
    print("3. ANGULAR ACCELERATION AND VELOCITY")
    print("-" * 45)
    
    # Taking moments about CG (from the solution)
    print("Taking moments about CG:")
    print("I_CG·α - 1200 × 1.0 - 400 × 2.5 = 0")
    
    # Moment arms from the solution (in meters)
    moment_vertical = R_vertical_main * 1.0  # 1200 kN × 1.0 m = 1200 kN·m
    moment_horizontal = R_horizontal_main * 2.5  # 400 kN × 2.5 m = 1000 kN·m
    
    total_moment = moment_vertical + moment_horizontal  # 2200 kN·m
    
    print(f"Moment from vertical force (1200 kN × 1.0 m): {moment_vertical/1e6:.1f} m·kN")
    print(f"Moment from horizontal force (400 kN × 2.5 m): {moment_horizontal/1e6:.1f} m·kN")
    print(f"Total moment: {total_moment/1e6:.1f} m·kN")
    
    # CORRECTED: Angular acceleration calculation
    # From the solution: α = 2200 × 10^6 / (5.65 × 10^8) = 3.9 rad/s²
    # The moment needs to be in consistent units with inertia
    # 2200 m·kN = 2200 × 10^6 N·mm
    # I_CG = 5.65 × 10^8 Ns²·mm
    
    total_moment_mm = total_moment * 1000  # Convert kN·m to N·mm (×1000 for m to mm)
    
    print(f"\nMoment in N·mm: {total_moment_mm:.3e}")
    print(f"Inertia in Ns²·mm: {I_cg_corrected:.3e}")
    
    alpha = total_moment_mm / I_cg_corrected
    
    print(f"\nAngular acceleration:")
    print(f"α = M / I_CG = {total_moment_mm:.3e} / {I_cg_corrected:.3e}")
    print(f"α = {alpha:.2f} rad/s²")
    
    # Angular velocity when vertical velocity becomes zero
    omega = alpha * t_vertical_zero
    print(f"\nAngular velocity at t = {t_vertical_zero:.4f} s:")
    print(f"ω = α × t = {alpha:.2f} × {t_vertical_zero:.4f} = {omega:.6f} rad/s")
    
    return {
        'mass': m,
        'vertical_inertia_force': F_vertical_inertia,
        'horizontal_inertia_force': F_horizontal_inertia,
        'vertical_acceleration': a_vertical,
        'horizontal_acceleration': a_horizontal,
        'time_zero_vertical_velocity': t_vertical_zero,
        'angular_velocity': omega,
        'angular_acceleration': alpha,
        'total_moment': total_moment
    }

def verify_calculation():
    """Direct verification of the angular acceleration calculation"""
    print("\n" + "="*60)
    print("DIRECT VERIFICATION OF ANGULAR ACCELERATION")
    print("="*60)
    
    # From the solution:
    total_moment = 2200e6  # 2200 m·kN = 2200 × 10^6 N·mm
    I_cg = 5.65e8  # Ns²·mm
    
    alpha = total_moment / I_cg
    
    print(f"Total moment: {total_moment:.3e} N·mm")
    print(f"Moment of inertia: {I_cg:.3e} Ns²·mm")
    print(f"Angular acceleration α = M/I = {total_moment:.3e} / {I_cg:.3e}")
    print(f"α = {alpha:.2f} rad/s²")
    
    # Check step by step
    print(f"\nStep-by-step calculation:")
    print(f"2200 × 10^6 / (5.65 × 10^8) = {2200/5.65:.3f} × 10^{6-8}")
    print(f"= {2200/5.65:.3f} × 10^{-2}")
    print(f"= {2200/(5.65*100):.3f}")
    print(f"= {2200/565:.3f} rad/s²")

def plot_results(results):
    """Create plots to visualize the results"""
    import matplotlib.pyplot as plt
    
    # Time array for plotting
    t_max = results['time_zero_vertical_velocity'] * 1.5
    t = np.linspace(0, t_max, 100)
    
    # Calculate vertical velocity over time (decelerating)
    v_vertical = 3.7 - results['vertical_acceleration'] * t
    
    # Calculate angular motion over time
    alpha = results['angular_acceleration']
    omega = alpha * t
    theta = 0.5 * alpha * t**2
    
    # Create figure with subplots
    fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(12, 10))
    
    # Plot 1: Vertical velocity vs time
    ax1.plot(t, v_vertical, 'b-', linewidth=2, label='Vertical velocity')
    ax1.axhline(y=0, color='r', linestyle='--', label='Zero velocity')
    ax1.axvline(x=results['time_zero_vertical_velocity'], color='g', linestyle='--', 
                label=f't = {results["time_zero_vertical_velocity"]:.3f} s')
    ax1.set_xlabel('Time (s)')
    ax1.set_ylabel('Vertical Velocity (m/s)')
    ax1.set_title('Vertical Velocity vs Time')
    ax1.grid(True, alpha=0.3)
    ax1.legend()
    
    # Plot 2: Angular velocity vs time
    ax2.plot(t, omega, 'g-', linewidth=2, label='Angular velocity')
    ax2.axvline(x=results['time_zero_vertical_velocity'], color='r', linestyle='--',
                label=f't = {results["time_zero_vertical_velocity"]:.3f} s')
    ax2.set_xlabel('Time (s)')
    ax2.set_ylabel('Angular Velocity (rad/s)')
    ax2.set_title('Angular Velocity vs Time')
    ax2.grid(True, alpha=0.3)
    ax2.legend()
    
    # Plot 3: Inertia forces
    forces = [results['vertical_inertia_force']/1000, 
              results['horizontal_inertia_force']/1000]
    force_labels = ['Vertical (950 kN)', 'Horizontal (400 kN)']
    
    colors = ['lightblue', 'lightcoral']
    bars = ax3.bar(force_labels, forces, color=colors, alpha=0.7)
    ax3.set_ylabel('Inertia Force (kN)')
    ax3.set_title('Inertia Forces')
    ax3.grid(True, alpha=0.3)
    
    # Add value labels on bars
    for bar, value in zip(bars, forces):
        ax3.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 10,
                f'{value:.0f}', ha='center', va='bottom', fontweight='bold')
    
    # Plot 4: Angular acceleration information
    text_str = f'Angular Acceleration:\nα = {results["angular_acceleration"]:.2f} rad/s²\n\nMoment = 2200 m·kN\nI_CG = 5.65×10^8 Ns²·mm'
    ax4.text(0.5, 0.5, text_str, transform=ax4.transAxes, fontsize=14,
            verticalalignment='center', horizontalalignment='center',
            bbox=dict(boxstyle="round,pad=0.3", facecolor="lightyellow"))
    ax4.set_xticks([])
    ax4.set_yticks([])
    ax4.set_title('Rotational Dynamics')
    
    plt.tight_layout()
    plt.show()

# Main execution
if __name__ == "__main__":
    # Run the analysis
    results = aircraft_analysis()
    
    # Verify the angular acceleration calculation
    verify_calculation()
    
    # Display final results
    print("\n" + "=" * 60)
    print("FINAL RESULTS")
    print("=" * 60)
    print(f"1. Inertia Forces:")
    print(f"   Vertical inertia force: {results['vertical_inertia_force']/1000:.0f} kN")
    print(f"   Horizontal inertia force: {results['horizontal_inertia_force']/1000:.0f} kN")
    print(f"   Vertical acceleration: {results['vertical_acceleration']/9.81:.1f} g")
    
    print(f"\n2. Time Analysis:")
    print(f"   Time for vertical velocity to become zero: {results['time_zero_vertical_velocity']:.4f} s")
    
    print(f"\n3. Rotational Analysis:")
    print(f"   Angular acceleration: {results['angular_acceleration']:.2f} rad/s²")
    print(f"   Angular velocity: {results['angular_velocity']:.6f} rad/s")
    
    # Ask user if they want to see plots
    plot_choice = input("\nDo you want to see graphical results? (y/n): ").lower()
    if plot_choice in ['y', 'yes']:
        try:
            plot_results(results)
            print("\nPlots displayed successfully!")
        except ImportError:
            print("Matplotlib not available. Please install it to view plots.")
        except Exception as e:
            print(f"Error generating plots: {e}")
    
    print("\nAnalysis completed successfully!")
