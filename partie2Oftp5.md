# TP4-Filtrage-Analogique
clear all
close all
clc
%cette fonction utiliser pour extraire la vecteur de audio musical
[music, fs] = audioread('test.wav');
%rendrela vecteur lineaire pour eviiter le probleme de dimension
music = music';
%extraire le length de audio
N = length(music);

f = (0:N-1)*(fs/N);
fshift = (-N/2:N/2-1)*(fs/N);
%appliquer la transformation de faurier directe
spectre_music = fft(music);
% afficher le spectre de signal pour visualiser les pics de bruit 
plot(fshift,fftshift(abs(spectre_music)));


k = 1;%ordre de filtrage ce paremtre nous permet de rendre le signal ideale aussi faire l'amplification
fc = 4500;%frequence de corpure 
%la transmitance complexe 
h = k./(1+1j*(f/fc).^100);

h_filter = [h(1:floor(N/2)), flip(h(1:floor(N/2)))];%on faire le choix de cet intervalle pour filter le signale et son symtrique par pour a fs/2
%faire le filtrage 
y_filtr = spectre_music(1:end-1).*h_filter;

sig_filtred= ifft(y_filtr,"symmetric");
%afficher la filtre (la transmettance complexe)
semilogx(f(1:floor(N/2)),abs( h(1:floor(N/2))),'linewidth',1.5)
%afficher le signal filter
plot(fshift(1:end-1),fftshift(abs(fft(sig_filtred))))
