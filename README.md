% Define Parameters
Pt_dBm = 30;   % Transmit power in dBm
Gt_dB = 10;    % Transmitter antenna gain in dB
Gr_dB = 10;    % Receiver antenna gain in dB
f = 2.4e9;     % Frequency in Hz (e.g., 2.4 GHz)
d_max = 10000; % Maximum distance in meters
d = linspace(100, d_max, 100);  % Distance range from 100 meters to d_max

% Constants
c = 3e8;       % Speed of light in m/s
receiver_sensitivity_dBm = -90; % Receiver sensitivity in dBm

% Free-Space Path Loss (FSPL)
Lfs = 20*log10(d) + 20*log10(f) + 20*log10(4*pi/c);  % Path loss in dB
Pr_dBm_fs = Pt_dBm + Gt_dB + Gr_dB - Lfs;  % Received power for FSPL

% Hata Model (for urban areas)
% Using Hata model for urban environments (in dB)
% Hata model parameters:
a_hata = 3.2 * (log10(11.75 * f))^2 - 4.97; % Correction factor for urban area
Lhata = 69.55 + 26.16 * log10(f) - 13.82 * log10(30) - a_hata + ...
        (44.9 - 6.55 * log10(30)) * log10(d); % Hata path loss model
Pr_dBm_hata = Pt_dBm + Gt_dB + Gr_dB - Lhata;  % Received power for Hata model

% ITU-R P.1546 Model (for satellite/long range communication)
% Using ITU-R P.1546 long-range propagation model (in dB)
Litu = 20*log10(d) + 20*log10(f) + 20*log10(4*pi/c) + 5*log10(d/1000) - 0.5;
Pr_dBm_itu = Pt_dBm + Gt_dB + Gr_dB - Litu;  % Received power for ITU-R P.1546

% Plotting the Results
figure;

% Plot Free-space Path Loss
subplot(2,2,1);
plot(d, Pr_dBm_fs, 'b', 'LineWidth', 2);
hold on;
plot(d, receiver_sensitivity_dBm * ones(size(d)), 'r--', 'LineWidth', 2);
title('Free-space Path Loss (FSPL)');
xlabel('Distance (m)');
ylabel('Received Power (dBm)');
legend('Received Power', 'Receiver Sensitivity');
grid on;

% Plot Hata Model (Urban Area)
subplot(2,2,2);
plot(d, Pr_dBm_hata, 'g', 'LineWidth', 2);
hold on;
plot(d, receiver_sensitivity_dBm * ones(size(d)), 'r--', 'LineWidth', 2);
title('Urban Area (Hata Model)');
xlabel('Distance (m)');
ylabel('Received Power (dBm)');
legend('Received Power', 'Receiver Sensitivity');
grid on;

% Plot ITU-R P.1546 Model (Satellite/Long-range)
subplot(2,2,3);
plot(d, Pr_dBm_itu, 'm', 'LineWidth', 2);
hold on;
plot(d, receiver_sensitivity_dBm * ones(size(d)), 'r--', 'LineWidth', 2);
title('Satellite/Long-range (ITU-R P.1546)');
xlabel('Distance (m)');
ylabel('Received Power (dBm)');
legend('Received Power', 'Receiver Sensitivity');
grid on;

% Plot All Models
subplot(2,2,4);
plot(d, Pr_dBm_fs, 'b', 'LineWidth', 2);
hold on;
plot(d, Pr_dBm_hata, 'g', 'LineWidth', 2);
plot(d, Pr_dBm_itu, 'm', 'LineWidth', 2);
plot(d, receiver_sensitivity_dBm * ones(size(d)), 'r--', 'LineWidth', 2);
title('Comparison of Path Loss Models');
xlabel('Distance (m)');
ylabel('Received Power (dBm)');
legend('Free-space Path Loss', 'Urban Area (Hata)', 'Satellite/Long-range (ITU-R)', 'Receiver Sensitivity');
grid on;
