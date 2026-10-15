Conseguir los datasets 2024 de Características, Matrícula y Trayectoria permitiría construir un diseño temporal más correcto:

Datos de 2024 → predicción de repitencia observada en 2025

Diseño recomendado
Predictores: datos 2024
Usaríamos:

infraestructura y conectividad 2024;
matrícula 2024;
trayectoria 2024;
Localizacion 2024;
provincia;
departamento;
sector;
ámbito;
antecedentes de repitencia, sobreedad o salidos sin pase de 2024.
Variable objetivo: datos 2025
No necesitaríamos obligatoriamente el archivo oficial de “Tasa de Repitencia”.

Podríamos calcular el target desde la Matrícula 2025:
tasa_repitencia_2025 =
repitentes_2025 / matrícula_2025 × 100

sí se puede descartar el XLSX de tasa de repitencia;

Dataset final:
cada fila representaria
provincia × departamento × sector × ámbito

Con esta estructura:

# Identificación

provincia
departamento
sector
ambito

# Predictores 2024

localizacion_2024
features_infraestructura_2024
matricula_2024
repitencia_2024 (calculada)
abandono_2024 (calculado)
sobreedad_2024
indicadores_trayectoria_2024

# Target 2025

matricula_2025
repitentes_2025
tasa_repitencia_2025

La columna tasa_repitencia_2025 debe quedar exclusivamente como variable objetivo, no como predictor.

Ventajas
Este diseño permite responder una pregunta mucho más clara:

¿Las condiciones de infraestructura, matrícula y trayectoria de 2024 permiten anticipar la repitencia de 2025?

También podríamos separar el aporte de cada grupo de variables:

Modelo ampliado
antecedentes educativos 2024

- infraestructura 2024
  → repitencia 2025

Precauciones importantes
Antes de implementarlo hay que verificar:

Que los archivos 2024 tengan la misma clave: - se cumple

Que esa clave también exista en los datos 2025. - se cumple

Que las definiciones de repitente, matrícula y trayectoria sean comparables entre años. - a primera vista tambien se cumple

Que no haya cambios fuertes en nombres de columnas, categorías o estructura educativa. - a cotejar con eda

Que los registros 2024 y 2025 puedan vincularse sin duplicaciones. - a cotejar con eda

Que los departamentos “Enmascarado” se traten consistentemente entre años.

¿Y la tasa oficial de repitencia?
Podríamos eliminarla del modelo principal y dejarla únicamente como validación externa opcional.

Es decir:

el target se calcula con Matrícula 2025;
el modelo se entrena con datos 2024;
la tasa oficial, si se conserva, se usa solo para controlar si el cálculo agregado coincide razonablemente con la publicación ministerial.
Conclusión

La formulación quedaría:

Predicción de la tasa de repitencia 2025 a partir de antecedentes educativos e infraestructura escolar observados durante 2024.

---- revisar punto 4.4
ver que variables vamos a tomar para la distribucion de la infraestructura
Sí. El dataset tiene varias variables potencialmente útiles, pero no conviene incorporar todas directamente. Primero hay que distinguir entre cobertura de establecimientos, cantidad de equipamiento y categorías institucionales.
Hallazgo sobre las variables actuales

- electricidad: tiene valores entre 98% y 99%, por lo que está casi saturada. Sirve como control descriptivo, pero probablemente discrimine poco entre grupos.
- internet_aulas: es más interesante porque varía entre 45% y 67%.
- laboratorio: muestra diferencias relevantes entre grupos.
- biblioteca: también tiene variabilidad útil.
  Variables adicionales recomendadas
  Conectividad por espacios
  Calcular como porcentaje sobre localizacion:
- espaciosconconexiónenlasaulas
- espaciosconconexiónenlabibliotecas
- espaciosconconexiónenellaboratorio
- espaciosconconexióneneláreadeadministración
- espaciosconconexiónenotroespaciodelestablecimiento
  Podés construir:
  indice_conectividad =
  promedio de las cinco coberturas
  También sería interesante comparar la conectividad administrativa con la pedagógica:
  brecha_conectividad =
  cobertura_administracion - cobertura_aulas
  Fuentes de energía
  Además de electricidadsi:
- electricidadredpública
- electricidadgrupoelectrógeno
- electricidadpanelfotovoltaicosolar
- electricidadgeneradoreólico
- electricidadgeneradorhidráulico
- electricidadotro
  Estas variables permitirían medir dependencia de fuentes alternativas:
  diversidad_energia =
  cantidad de fuentes de energía disponibles
  Las variables eólica e hidráulica probablemente sean muy poco frecuentes, por lo que primero hay que revisar su variabilidad.
  Equipamiento institucional
  Estas variables son conteos de aparatos, no porcentajes de establecimientos:
- televisores;
- proyectores o cañones;
- scanners;
- webcams;
- impresoras;
- servidores;
- impresoras 3D;
- equipos de sonido;
- pizarras digitales.
  No las uses directamente. Normalizalas:
  equipamiento_por_establecimiento =
  cantidad_de_aparatos / localizacion
  Y, si tienen mucha asimetría:
  equipamiento_transformado = np.log1p(equipamiento_por_establecimiento)
  Podrías construir un índice reducido:
  indice_equipamiento =
  promedio estandarizado de los equipamientos normalizados
  Biblioteca y laboratorio
  Además de saber si existen, conviene medir si son espacios exclusivos:
- bibliotecadisponedealmenosunasi
- bibliotecafuncionaesespacioexclusivosi
- disponedesalaolaboratoriodeinformáticasi
- laboratoriofuncionaesespacioexclusivosi
  Esto distingue entre “la institución tiene el espacio” y “el espacio funciona realmente como infraestructura especializada”.
  Gestión escolar
  Podés incluir:
- sistemadegestiónescolarsi
- sistemadegestiónescolarprovidoporelministerio
- sistemadegestiónescolardesarrolladporterceros
- sistemadegestiónescolarencargadoporelestablecimiento
- sistemadegestiónescolarplanilladecalculo
  Pero no uses simultáneamente sistemadegestiónescolarsi y todas sus modalidades específicas. Elegí una de estas alternativas:
  Opción simple:
  sistemadegestiónescolarsi
  Opción detallada:
  modalidades específicas, dejando una como categoría de referencia
  Cooperadora
  Las categorías:
- cooperadoraconpersoneríajurídica
- cooperadorasinpersoneríajurídica
- cooperadorano
  son mutuamente excluyentes. Para un modelo, no incluyas las tres juntas con intercepto. Dejá una como referencia.
  Conjunto reducido que recomiendo
  Para el EDA y un primer modelo usaría:

1.  electricidadsi
2.  indice_conectividad_espacios
3.  internet_aulas
4.  bibliotecadisponedealmenosunasi
5.  bibliotecafuncionaesespacioexclusivosi
6.  disponedesalaolaboratoriodeinformáticasi
7.  laboratoriofuncionaesespacioexclusivosi
8.  indice_equipamiento_establecimiento
9.  indice_equipamiento_biblioteca
10. sistemadegestiónescolarsi
11. Una o dos variables de cooperadora.
12. np.log1p(localizacion) como variable de control de tamaño.
    Precauciones

- No mezclar conteos brutos de equipamiento con localizacion.
- No usar todas las variables originales y todos los índices compuestos a la vez.
- Revisar columnas completamente vacías o casi constantes.
- No convertir automáticamente todos los faltantes en cero sin verificar que un blanco realmente signifique ausencia.
- Mantener todas estas variables como predictores de 2024.
- Reservar la repitencia 2025 exclusivamente para construir el objetivo.
  La ampliación más valiosa, en términos prácticos, sería: conectividad por espacios + equipamiento normalizado + existencia y exclusividad de biblioteca/laboratorio. Estas variables tienen más capacidad de diferenciar grupos que electricidad, que aparece prácticamente universalizada.
