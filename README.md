% Given data
h_coeff = 20; % heat transfer coefficient, W/m^2K
T_inf = 25; % Ambient temperature, °C
T0 = 300; % Initial temperature, °C
R = 0.05; % Radius of the sphere in meters
rho = 7800; % Density of iron in kg/m^3
cp = 450; % Specific heat capacity of iron in J/(kg*K)

% Time parameters
t_initial = 0;
t_final = 1000;
dt = 100; % Step size in seconds

% Calculated cooling constant
k = (3 * h_coeff) / (rho * cp * R);

% Differential equation as a function handle
dTdt = @(t, T) -k * (T - T_inf);

% Initialize variables for RK4
t = t_initial:dt:t_final;
T = zeros(size(t));
T(1) = T0; % Initial condition

% Fourth-Order Runge-Kutta Method
for i = 1:(length(t)-1)
    k1 = dt * dTdt(t(i), T(i));
    k2 = dt * dTdt(t(i) + dt/2, T(i) + k1/2);
    k3 = dt * dTdt(t(i) + dt/2, T(i) + k2/2);
    k4 = dt * dTdt(t(i) + dt, T(i) + k3);
    
    T(i+1) = T(i) + (k1 + 2*k2 + 2*k3 + k4) / 6;
end

% Display results
fprintf('Temperature profile using RK4 method:\n');
disp(table(t', T', 'VariableNames', {'Time_sec', 'Temperature_C'}));

% Solve using MATLAB's ode45 for comparison
[t_ode, T_ode] = ode45(dTdt, [t_initial, t_final], T0);

% Plot the results
figure;
plot(t, T, 'o-', 'LineWidth', 1.5);
hold on;
plot(t_ode, T_ode, 'r-', 'LineWidth', 1.5);
xlabel('Time (s)');
ylabel('Temperature (°C)');
title('Cooling of Metal Sphere');
legend('RK4 Method', 'ode45 Solution');
grid on;
# mat
