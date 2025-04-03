import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

# Leer el archivo Excel excluyendo las 2 primeras filas
df = pd.read_excel(r'C:\Users\rafad\Downloads\Datos wifi.xlsx', 
                  sheet_name='Hoja2',
                  skiprows=2)  # Excluye las primeras 2 filas

# Corregir el valor atípico en la distancia 5 (ajustando a -67.36)
df.loc[df['Distancia'] == 5, 'Potencia (dBm)'] = -67.36

# Crear grupos por colores según la tabla
def asignar_grupo(distancia):
    if distancia <= 1:
        return 'Amarillo (0-1m)'
    elif distancia <= 5:
        return 'Rojo (2-5m)'
    elif distancia <= 13:
        return 'Verde (6-13m)'
    else:
        return 'Morado (14-17m)'

df['Grupo'] = df['Distancia'].apply(asignar_grupo)

# Definir el orden de los grupos y colores
orden_grupos = ['Amarillo (0-1m)', 'Rojo (2-5m)', 'Verde (6-13m)', 'Morado (14-17m)']
colores = {
    'Amarillo (0-1m)': 'gold',
    'Rojo (2-5m)': 'red',
    'Verde (6-13m)': 'green',
    'Morado (14-17m)': 'purple'
}

# Crear figura con dos subplots
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(14, 12), gridspec_kw={'height_ratios': [2, 1]})

# 1. Crear boxplot agrupado por rangos de colores
sns.boxplot(data=df, x='Grupo', y='Potencia (dBm)', ax=ax1, 
            order=orden_grupos,
            palette=[colores[grupo] for grupo in orden_grupos])

ax1.set_title('Distribución de Potencia por Grupos de Distancia')
ax1.set_xlabel('Grupos de Distancia')
ax1.set_ylabel('Potencia (dBm)')
ax1.grid(True, linestyle='--', alpha=0.3)

# 2. Gráfico de líneas para la Potencia y Desviación Estándar por grupos
grupo_stats = df.groupby('Grupo')['Potencia (dBm)'].agg(['mean', 'std']).reset_index()
grupo_stats['Desviación'] = df.groupby('Grupo')['Desviación estandar (dBm)'].mean().values

# Convertir grupos a una escala numérica para el gráfico de líneas
grupo_stats['Grupo_num'] = [0, 1, 2, 3]  # Asignar valores numéricos para los grupos en orden

# Graficar línea de potencia por grupos
ax2.plot(grupo_stats['Grupo_num'], grupo_stats['mean'], 'o-', color='blue', label='Potencia Media (dBm)')

# Graficar barras de error con la desviación estándar
ax2.errorbar(grupo_stats['Grupo_num'], grupo_stats['mean'], 
             yerr=grupo_stats['Desviación'], fmt='o', capsize=5, 
             color='blue', alpha=0.7)

# Ajustar etiquetas del eje X
ax2.set_xticks(grupo_stats['Grupo_num'])
ax2.set_xticklabels(orden_grupos)

# Colorear el fondo según los grupos
for i, grupo in enumerate(orden_grupos):
    ax2.axvspan(i-0.5, i+0.5, alpha=0.2, color=colores[grupo])

ax2.set_title('Potencia vs Grupos de Distancia con Desviación Estándar')
ax2.set_xlabel('Grupos de Distancia')
ax2.set_ylabel('Potencia (dBm)')
ax2.legend()
ax2.grid(True, linestyle='--', alpha=0.3)

plt.tight_layout()
plt.show()

# Mostrar estadísticas por grupo
print("\nEstadísticas por grupo:")
for grupo in orden_grupos:
    grupo_df = df[df['Grupo'] == grupo]
    print(f"\n{grupo}:")
    print(f"  Potencia media: {grupo_df['Potencia (dBm)'].mean():.2f} dBm")
    print(f"  Desviación estándar media: {grupo_df['Desviación estandar (dBm)'].mean():.2f} dBm")
    print(f"  Rango de potencia: {grupo_df['Potencia (dBm)'].min():.2f} a {grupo_df['Potencia (dBm)'].max():.2f} dBm")
    print(f"  Distancias incluidas: {sorted(grupo_df['Distancia'].unique())}")
