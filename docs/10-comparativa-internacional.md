# 10 · Comparativa internacional

**Fecha de corte: 10 de septiembre de 2026**

---

## 1. Marco regulatorio

| | 🇬🇧 Reino Unido | 🇪🇺 Unión Europea | 🇺🇸 EE.UU. | 🇧🇷 Brasil | 🇨🇱 Chile | 🇦🇺 Australia | 🇮🇳 India | 🇨🇴 **Colombia** |
|---|---|---|---|---|---|---|---|---|
| **Norma base** | CMA Order 2017 + DUAA 2025 | PSD2 → PSD3/PSR; FIDA (propuesta) | Dodd-Frank §1033 | Res. Conjunta 1/2020 BCB | Ley 21.521 (Fintec) | CCA 2010 Part IVD | Directrices RBI (NBFC-AA) | **Ley 2294/2023 art. 89 + D-0368/2026** |
| **Regulador** | FCA / PSR / CMA | Comisión / EBA / ANC | CFPB | BCB | CMF | ACCC / OAIC / DSB | RBI | **SFC** (+URF, MinHacienda) |
| **Obligatorio** | Sí (CMA9) | Sí | Formalmente sí, **suspendido judicialmente** | Sí (universal) | Sí | Sí | Sí | **Sí (universo vigilado)** |
| **Alcance** | Cuentas de pago → open finance | Cuentas de pago (FIDA ampliaría) | Tarjetas de crédito + cuentas Reg E | Open finance completo | Open finance | Multisectorial | Financiero amplio | **Open finance + portabilidad** |
| **Iniciación de pagos** | Sí (VRP/cVRP) | Sí (PIS) | Indirecta | Sí (Pix + JSR) | Sí (PSIP) | *Action initiation* en reforma | No | **Sí (Título 4 Libro 17, desde 2022); estándares SFC pendientes** |
| **Titulares persona jurídica** | Sí (cuentas de negocio) | Limitado | No (solo consumidor) | Sí | Sí | Sí | Sí | **Sí, explícito** |

## 2. Modelo técnico

| | Reino Unido | UE | EE.UU. | Brasil | Australia | **Colombia** |
|---|---|---|---|---|---|---|
| **Estándar de API** | OBL Standard v4.0 | Berlin Group / STET (no único) | FDX API | Open Finance Brasil | CDR Standards (DSB) | **Por definir (SFC)** |
| **Perfil de seguridad** | FAPI 1.0 Advanced | FAPI de facto | FAPI (FDX) | **FAPI** | FAPI 1.0 Advanced | **FAPI 2.0 (obligatorio desde CE 004/2024)** |
| **Formato / diseño** | REST/JSON, OpenAPI | REST/JSON | REST/JSON | REST/JSON, OpenAPI | REST/JSON | **REST/JSON, OpenAPI 3.1** |
| **Diccionario de datos** | Propio | Propio | FDX | Propio | Propio | **ISO 20022** (+ACORD/ISIN/CFI/OpenFunds/FIX) |
| **PKI** | eIDAS / OBWAC-OBSeal | eIDAS QWAC/QSeal | Certificados privados | **ICP-Brasil** | Certificados CDR | **Ley 527 de 1999** |
| **Directorio** | OB Directory (OBL) | Registros nacionales + EBA | No central | Diretório de Participantes | CDR Register | **Directorio de la SFC** |
| **Certificación obligatoria** | Sí | No uniforme | No | **Sí** | **Sí** | **No (espacio de pruebas facultativo)** |

## 3. Modelo económico y de acceso

| | Reino Unido | UE | EE.UU. | Brasil | **Colombia** |
|---|---|---|---|---|---|
| **Costo del dato** | Gratis (obligatorio); cVRP con tarifa vía esquema | Gratis (PSD2); FIDA introduciría compensación | Gratis hoy; el NPRM abriría tarifas tras N solicitudes | Gratis | **Gratis** |
| **Costo de infraestructura** | Vía esquema comercial (UKPI) | En discusión (FIDA/FDSS) | En discusión | No se cobra | **Recuperación de costos por volumen** |
| **Auditor de tarifas** | Operador del esquema | FDSS (propuesto) | Por definir | — | ❌ **No designado** |
| **Entrada de no regulados** | Autorización FCA (AISP/PISP) | Licencia AISP/PISP | Agregadores privados / bilateral | Registro ante BCB | ⚠️ **Solo acuerdo bilateral voluntario** |
| **Niveles de acreditación** | No | No | No | No | No — **Australia sí (modelo a considerar)** |

## 4. Consentimiento

| | Modelo | Dashboard obligatorio | Doble verificación |
|---|---|---|---|
| Reino Unido | Consentimiento ante el TPP; autenticación ante el ASPSP | Sí (acceso vía banca) | No |
| UE (PSD2/PSR) | Consentimiento ante el TPP + SCA | Sí (PSR lo refuerza) | No |
| EE.UU. (§1033) | Autorización expresa al tercero, renovable anualmente | Sí | No |
| Brasil | API de *consents* con ciclo de vida | Sí | No |
| India | **Artefacto de consentimiento** legible por máquina (DEPA) | Sí | No |
| **Colombia** | **Autorización** ante el Tercero (art. 2.35.8.3.2) **+ Confirmación** ante el Proveedor (art. 2.35.8.3.3) | Implícito en los derechos del art. 2.35.8.3.5 | ✅ **Sí — caso único** |

## 5. Estado de madurez y escala

| País | Usuarios / conexiones | Volumen | Estado |
|---|---|---|---|
| 🇧🇷 Brasil | **100M+ clientes** | Mayor ecosistema obligatorio del mundo | 🟢 Maduro |
| 🇬🇧 Reino Unido | **18,81M** conexiones (jun-2026); ~17M usuarios activos | **2,81 mil millones** de llamadas API/mes; **1.000M+ pagos** acumulados; 40,16M pagos/mes | 🟢 Maduro |
| 🇦🇺 Australia | Millones | Multisectorial | 🟢 Maduro |
| 🇮🇳 India | Cientos de millones de cuentas | Dominado por crédito a microempresas | 🟢 Alta escala |
| 🇪🇺 UE | Decenas de millones | Fragmentado por país | 🟡 Maduro pero desigual |
| 🇨🇱 Chile | — | Prorrogado a jul-2027 | 🟡 Preoperativo |
| 🇨🇴 **Colombia** | — | Estándares por expedir | 🟠 **Arrancando** |
| 🇺🇸 EE.UU. | Millones vía agregadores | Todo bilateral | 🔴 Limbo judicial |
| 🇲🇽 México | — | Solo datos abiertos | 🔴 Estancado |

## 6. Dónde queda Colombia — lectura estratégica

### Lo que Colombia hizo bien
1. **Habilitación legal expresa** (art. 89 de la Ley 2294 de 2023). Es lo que le falta a
   EE.UU. y por lo que su regla está suspendida.
2. **Alcance amplio desde el día uno**: depósito, crédito, inversión, seguros, KYC y
   comparación de oferta. La UE lleva 3 años sin lograr esto con FIDA.
3. **Personas jurídicas incluidas** como titulares — cubre a las pymes, que es donde está
   el mayor problema de acceso a crédito.
4. **FAPI 2.0 directamente**, sin pasar por 1.0, y desde febrero de 2024. Correcto según la
   propia recomendación de la OpenID Foundation, y **antes** que Reino Unido, Brasil y
   Australia, que siguen en FAPI 1.0 Advanced. Es la mayor fortaleza técnica del marco colombiano.
5. **Cláusula de obsolescencia automática** de estándares — evita quedarse anclado.
6. **Portabilidad financiera** como servicio del sistema (D-0977/2026): un caso de uso con
   beneficio tangible y medible para el consumidor. Brasil llegó a esto en 2026, seis años
   después de arrancar.

### Lo que queda flojo frente a los referentes
1. **Cronograma muy largo** — 11 años para iniciación de pagos.
2. **Sin derecho de acceso para no vigilados en finanzas abiertas**, y sin niveles de
   acreditación intermedios (en iniciación de pagos sí lo hay).
3. **Sin auditor de tarifas** de infraestructura.
4. **Certificación de conformidad facultativa**, no requisito de producción.
5. **Concentración institucional**: la SFC es regulador, operador del directorio, fijador
   de estándares y supervisor. En UK y Brasil esas funciones están separadas.
6. **Sin modelo comercial** para el caso de pagos — el problema que el Reino Unido tardó
   ocho años en resolver y que resolvió creando un esquema privado (UKPI).
