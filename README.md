[ejercicio1 A Y B.py](https://github.com/user-attachments/files/34554846/ejercicio1.A.Y.B.py)
def limpiar_registro_sensor(registro_raw):
    """
    Procesa un diccionario con datos crudos de un sensor y retorna 
    un diccionario estandarizado.
    """
    # Si no es un diccionario, el mensaje te dirá qué es exactamente lo que recibió
    if not isinstance(registro_raw, dict):
        raise TypeError(f"Se esperaba un diccionario, pero se recibió un tipo: {type(registro_raw)}")

    # 1. sensor_id: limpiar espacios sobrantes
    sensor_id_raw = registro_raw.get("sensor_id", "")
    sensor_id = str(sensor_id_raw).strip() if sensor_id_raw else "SIN_ID"

    # 2. temperatura: manejar comas como separadores decimales y extraer float
    temp_raw = str(registro_raw.get("temperatura", 0.0)).replace(",", ".")
    temp_filtrada = "".join([c for c in temp_raw if c.isdigit() or c in ".-"])
    
    try:
        temperatura = float(temp_filtrada) if temp_filtrada else 0.0
    except ValueError:
        temperatura = 0.0

    # 3. ubicacion: valor por defecto seguro si no existe o está vacía
    ubicacion_raw = registro_raw.get("ubicacion")
    if not ubicacion_raw or not isinstance(ubicacion_raw, str) or not ubicacion_raw.strip():
        ubicacion = "Planta No Asignada"
    else:
        ubicacion = ubicacion_raw.strip()

    return {
        "sensor_id": sensor_id,
        "temperatura": temperatura,
        "ubicacion": ubicacion
    }


# --- BLOQUE PRINCIPAL ---
if __name__ == "__main__":
    dato_crudo = {
        "sensor_id": " SENSOR-001 ",
        "temperatura": "24,5C",
        "ubicacion": " "
    }

    registro_estandarizado = limpiar_registro_sensor(dato_crudo)
    print("Resultado exitoso:", registro_estandarizado)

###############EJERCICIO B#####################################
def limpiar_registro_sensor(registro_raw):
    """
    Procesa un diccionario con datos crudos de un sensor y retorna 
    un diccionario estandarizado.
    """
    # Si no es un diccionario, el mensaje te dirá qué es exactamente lo que recibió
    if not isinstance(registro_raw, dict):
        raise TypeError(f"Se esperaba un diccionario, pero se recibió un tipo: {type(registro_raw)}")

    # 1. sensor_id: limpiar espacios sobrantes
    sensor_id_raw = registro_raw.get("sensor_id", "")
    sensor_id = str(sensor_id_raw).strip() if sensor_id_raw else "SIN_ID"

    # 2. temperatura: manejar comas como separadores decimales y extraer float
    temp_raw = str(registro_raw.get("temperatura", 0.0)).replace(",", ".")
    temp_filtrada = "".join([c for c in temp_raw if c.isdigit() or c in ".-"])
    
    try:
        temperatura = float(temp_filtrada) if temp_filtrada else 0.0
    except ValueError:
        temperatura = 0.0

    # 3. ubicacion: valor por defecto seguro si no existe o está vacía
    ubicacion_raw = registro_raw.get("ubicacion")
    if not ubicacion_raw or not isinstance(ubicacion_raw, str) or not ubicacion_raw.strip():
        ubicacion = "Planta No Asignada"
    else:
        ubicacion = ubicacion_raw.strip()

    return {
        "sensor_id": sensor_id,
        "temperatura": temperatura,
        "ubicacion": ubicacion
    }


# --- BLOQUE PRINCIPAL ---
if __name__ == "__main__":
    dato_crudo = {
        "sensor_id": " SENSOR-001 ",
        "temperatura": "24,5C",
        "ubicacion": " "
    }

    registro_estandarizado = limpiar_registro_sensor(dato_crudo)
    print("Resultado exitoso:", registro_estandarizado)

def limpiar_registros_crudos(registros_crudos):
    lista_limpia = []
    for reg in registros_crudos:
        try:
            sensor_id = str(reg.get("id", "")).strip().upper()
            temp_str = str(reg.get("temperatura", "0")).replace("°C", "").strip()
            temperatura = float(temp_str)
            registro_sanitizado = {
                "id": sensor_id,
                "temperatura": temperatura,
                "estado": reg.get("estado", "desconocido").strip().lower()
            }
            lista_limpia.append(registro_sanitizado)
        except (ValueError, TypeError):
            continue
    return lista_limpia

def generar_resumen_sensores(lista_sensores_limpios, umbral_temperatura):
      if not  lista_sensores_limpios:
        return [], 0.0
      lista_sensores_alerta = []
      suma_temperaturas = 0.0
      total_sensores = len(lista_sensores_limpios)
      for sensor in lista_sensores_limpios:
          temp = sensor["temperatura"]
          suma_temperaturas += temp
          if temp > umbral_temperatura:
              lista_sensores_alerta.append(sensor["id"])
              promedio_temperatura = suma_temperaturas / total_sensores
              return lista_sensores_alerta, promedio_temperatura
if __name__ == "__main__":
        registros_crudos_planta = [
        {"id": "sen-01", "temperatura": " 65.4°C", "estado": " ACTIVO "},
        {"id": "sen-02", "temperatura": " 42.0°C", "estado": " ACTIVO "},
        {"id": "sen-03", "temperatura": " 78.9°C", "estado": " ACTIVO "},
        {"id": "sen-04", "temperatura": " 55.1°C", "estado": " INACTIVO"},
        {"id": "sen-05", "temperatura": " 81.2°C", "estado": " ACTIVO "},
    ]
print("--- 1. PROCESO DE LIMPIEZA (Punto A) --- ")
sensores_limpios = limpiar_registros_crudos(registros_crudos_planta)
for s in sensores_limpios:
    print(s)
UMBRAL_LIMITE = 70.0
print(f"\n--- 2. GENERANDO RESUMEN (Umbral: {UMBRAL_LIMITE} °C)---")
sensores_alerta, promedio_global = generar_resumen_sensores(sensores_limpios, UMBRAL_LIMITE)
print("\n" + "="*40)
print("    REPORTE TÉRMICO DE PLANTA")
print("="*40)
print(f". Total de dispositivos analizados : {len(sensores_limpios)}")
print(f". Promedio térmico global de planta : {promedio_global:.2f} °C")
print(f". Umbral de seguridad configurado : {UMBRAL_LIMITE} °C")
print(f". Dispositivos en estado de alerta : {len(sensores_alerta)}")
if sensores_alerta:
    print("\n[!] Dispositivos que superan el umbral:")
    for dev_id in sensores_alerta:
        print(f"     - ID: {dev_id}")
    else: 
        print("\n[✔] Ningún dispositivo supera el umbral de alerta.")
        print("="*40)
