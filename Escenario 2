import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# Leer el archivo Excel - Corregido para usar la hoja correcta
df = pd.DataFrame({
    'Distancia': [3, 0, 1, 2, 4, 5, 6, 17, 18, 7, 19, 8, 16, 9, 10, 14, 13, 12, 15, 11, 20],
    'Potencia (dBm)': [-42.32, -55.65, -56.49, -62.61, -64.14, -66.68, -69.79, -69.85, -70.4, -70.81, 
                        -71.69, -72.64, -72.83, -73.96, -75.25, -77.15, -77.24, -77.53, -78.4, -78.74, -81.75],
    'Desviación estandar (dBm)': [2.42, 1.19, 0.87, 3.33, 4.87, 3.46, 1.12, 0.37, 0.79, 4.1, 
                                    0.93, 1.02, 0.78, 0.83, 1.09, 1.1, 1.32, 0.67, 0.73, 2.34, 2.54]
})
print("Usando datos de ejemplo basados en la imagen proporcionada")

# Para el Escenario 2, ajustamos los grupos según la tabla proporcionada
def asignar_grupo(distancia):
    if distancia == 3:
        return 'Rojo (3m)'
    elif distancia <= 1:
        return 'Amarillo (0-1m)'
    elif distancia == 2 or (distancia >= 4 and distancia <= 6) or distancia == 17:
        return 'Verde (2m, 4-6m, 17m)'
    elif (distancia >= 7 and distancia <= 16) or (distancia >= 18 and distancia <= 19):
        return 'Azul (7-16m, 18-19m)'
    elif distancia == 20:
        return 'Morado (20m)'
    else:
        return 'Otro'

df['Grupo'] = df['Distancia'].apply(asignar_grupo)

# Definir el orden de los grupos y colores
orden_grupos = ['Rojo (3m)', 'Amarillo (0-1m)', 'Verde (2m, 4-6m, 17m)', 'Azul (7-16m, 18-19m)', 'Morado (20m)']
colores = {
    'Rojo (3m)': 'red',
    'Amarillo (0-1m)': 'gold',
    'Verde (2m, 4-6m, 17m)': 'green',
    'Azul (7-16m, 18-19m)': 'blue',
    'Morado (20m)': 'purple'
}

# Crear figura con dos subplots
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(14, 12), gridspec_kw={'height_ratios': [2, 1]})

# 1. Crear boxplot agrupado por rangos de colores
sns.boxplot(data=df, x='Grupo', y='Potencia (dBm)', ax=ax1, 
            order=orden_grupos,
            palette=[colores[grupo] for grupo in orden_grupos])

ax1.set_title('Distribución de Potencia por Grupos de Distancia - Escenario 2')
ax1.set_xlabel('Grupos de Distancia')
ax1.set_ylabel('Potencia (dBm)')
ax1.tick_params(axis='x', rotation=45)  # Rotar etiquetas para mejor legibilidad
ax1.grid(True, linestyle='--', alpha=0.3)

# 2. Gráfico de líneas para la Potencia y Desviación Estándar por grupos
grupo_stats = df.groupby('Grupo')['Potencia (dBm)'].agg(['mean', 'std']).reset_index()
grupo_stats['Desviación'] = df.groupby('Grupo')['Desviación estandar (dBm)'].mean().values

# Reordenar los datos según el orden deseado
grupo_stats['orden'] = grupo_stats['Grupo'].map({grupo: i for i, grupo in enumerate(orden_grupos)})
grupo_stats = grupo_stats.sort_values('orden')

# Graficar línea de potencia por grupos
ax2.plot(range(len(grupo_stats)), grupo_stats['mean'], 'o-', color='darkblue', label='Potencia Media (dBm)')

# Graficar barras de error con la desviación estándar
ax2.errorbar(range(len(grupo_stats)), grupo_stats['mean'], 
             yerr=grupo_stats['Desviación'], fmt='o', capsize=5, 
             color='darkblue', alpha=0.7)

# Ajustar etiquetas del eje X
ax2.set_xticks(range(len(grupo_stats)))
ax2.set_xticklabels(grupo_stats['Grupo'], rotation=45)

# Colorear el fondo según los grupos
for i, grupo in enumerate(grupo_stats['Grupo']):
    ax2.axvspan(i-0.5, i+0.5, alpha=0.2, color=colores[grupo])

ax2.set_title('Potencia vs Grupos de Distancia con Desviación Estándar - Escenario 2')
ax2.set_xlabel('Grupos de Distancia')
ax2.set_ylabel('Potencia (dBm)')
ax2.legend()
ax2.grid(True, linestyle='--', alpha=0.3)

plt.tight_layout()
plt.show()

# Mostrar estadísticas por grupo
print("\nEstadísticas por grupo - Escenario 2:")
for grupo in orden_grupos:
    grupo_df = df[df['Grupo'] == grupo]
    if not grupo_df.empty:
        print(f"\n{grupo}:")
        print(f"  Potencia media: {grupo_df['Potencia (dBm)'].mean():.2f} dBm")
        print(f"  Desviación estándar media: {grupo_df['Desviación estandar (dBm)'].mean():.2f} dBm")
        print(f"  Rango de potencia: {grupo_df['Potencia (dBm)'].min():.2f} a {grupo_df['Potencia (dBm)'].max():.2f} dBm")
        print(f"  Distancias incluidas: {sorted(grupo_df['Distancia'].unique())}")
