classDiagram
    %% Clase Principal Hospital
    class Hospital {
        -nombreHospital: String
        -direccion: String
        -telefono: String
        -email: String
        -capacidadCamas: int
        -doctores: List~Doctor~
        -pacientes: List~Paciente~
        -farmacias: List~Farmacia~
        -departamentos: List~Departamento~
        +agregarPaciente(paciente: Paciente)
        +agregarDoctor(doctor: Doctor)
        +eliminarPaciente(idPaciente: String)
        +eliminarDoctor(idDoctor: String)
        +buscarPaciente(idPaciente: String): Paciente
        +buscarDoctor(idDoctor: String): Doctor
        +gestionarCitas()
        +generarReporteOcupacion(): Report
        +listarDoctores(): List~Doctor~
    }

    %% Clase Usuario Base
    class Usuario {
        <<abstract>>
        -idUsuario: String
        -nombre: String
        -apellido: String
        -direccion: String
        -telefono: String
        -email: String
        -fechaNacimiento: Date
        -genero: String
        +getNombre(): String
        +setNombre(nombre: String)
        +iniciarSesion()
        +cerrarSesion()
        +actualizarPerfil()
    }

    %% Gestor de Archivos y Autenticación
    class GestorArchivos {
        -baseDatos: String
        +guardarUsuario(usuario: Usuario)
        +leerUsuarios(tipo: String)
        +validarCredenciales(idUsuario: String): Usuario
        +validarClaveSecreta(clave: String): boolean
        +actualizarUsuario(usuario: Usuario)
        +eliminarUsuario(idUsuario: String)
        +generarBackup()
        +restaurarBackup()
    }

    %% Clase Estado Paciente
    class EstadoPaciente {
        -estadoActual: String
        +ACTIVO: String
        +HOSPITALIZADO: String
        +ALTA: String
        +FALLECIDO: String
        +AMBULATORIO: String
        +getEstado(): String
        +setEstado(estado: String)
    }

    %% Clase Paciente
    class Paciente {
        -numeroHistoriaClinica: String
        -grupoSanguineo: String
        -alergias: List~String~
        -fechaNacimiento: Date
        -seguroMedico: String
        -estadoPaciente: EstadoPaciente
        -doctorAsignado: Doctor
        +solicitarCita(doctor: Doctor, fecha: Date): Cita
        +verHistorial(): List~ExpedienteMedico~
        +actualizarSeguro(seguro: String)
        +agregarAlergia(alergia: String)
        +getEstado(): EstadoPaciente
    }



    %% Clase Doctor
    class Doctor {
        -especialidad: String
        -numeroLicencia: String
        -añosExperiencia: int
        -departamento: Departamento
        -horariosDisponibles: List~Horario~
        -pacientesAsignados: List~Paciente~
        +diagnosticar(paciente: Paciente): Diagnostico
        +recetarMedicamentos(paciente: Paciente): Prescripcion
        +solicitarExamen(paciente: Paciente, tipo: String): Examen
        +actualizarExpediente(expediente: ExpedienteMedico)
        +verAgenda(): List~Cita~
        +agregarHorario(horario: Horario)
    }

    %% Clase Enfermera
    class Enfermera {
        -departamento: String
        -turno: String
        -certificaciones: List~String~
        +administrarMedicacion(paciente: Paciente, medicamento: Medicamento)
        +tomarSignosVitales(paciente: Paciente): SignosVitales
        +registrarEvolucion(paciente: Paciente, notas: String)
        +asistirCirugia(cirugia: Cirugia)
    }

    %% Clase Administrador
    class Administrador {
        -areaGestion: String
        -nivelAcceso: int
        +gestionarEmpleados(): List~Usuario~
        +generarReportes(): Report
        +gestionarInventario(): Inventario
        +aprobarSolicitudes(solicitud: Solicitud)
        +gestionarPresupuesto()
        +asignarRecursos()
    }

    %% Clase Departamento
    class Departamento {
        -idDepartamento: String
        -nombreDepartamento: String
        -jefeDepartamento: Doctor
        -personal: List~Usuario~
        -ubicacion: String
        -presupuesto: double
        +agregarPersonal(usuario: Usuario)
        +removerPersonal(idUsuario: String)
        +generarReporteDepartamento(): Report
        +solicitarRecursos(recursos: List~String~)
    }

    %% Clase Cita Medica
    class CitaMedica {
        -idCita: String
        -paciente: Paciente
        -doctor: Doctor
        -fechaHora: DateTime
        -motivo: String
        -estado: EstadoCita
        -consultorio: String
        -duracionMinutos: int
        +programarCita()
        +cancelarCita()
        +reprogramarCita(nuevaFecha: DateTime)
        +confirmarAsistencia()
        +getEstado(): EstadoCita
    }



    %% Clase Expediente Medico
    class ExpedienteMedico {
        -idExpediente: String
        -paciente: Paciente
        -diagnosticos: List~Diagnostico~
        -prescripciones: List~Prescripcion~
        -examenes: List~Examen~
        -cirugias: List~Cirugia~
        -alergias: List~String~
        +agregarDiagnostico(diagnostico: Diagnostico)
        +agregarPrescripcion(prescripcion: Prescripcion)
        +obtenerHistorialCompleto(): String
        +exportarPDF(): File
        +buscarPorFecha(fecha: Date): List
    }

    %% Clase Diagnostico
    class Diagnostico {
        -idDiagnostico: String
        -fecha: Date
        -descripcion: String
        -codigoCIE10: String
        -doctor: Doctor
        -gravedad: String
        -tratamiento: String
        +actualizarDiagnostico()
        +agregarNotas(notas: String)
    }

    %% Clase Prescripcion
    class Prescripcion {
        -idPrescripcion: String
        -fecha: Date
        -medicamentos: List~MedicamentoPrescrito~
        -instrucciones: String
        -duracionDias: int
        -doctor: Doctor
        -activa: boolean
        +agregarMedicamento(medicamento: MedicamentoPrescrito)
        +suspenderPrescripcion()
        +renovarPrescripcion()
    }

    %% Clase Medicamento Prescrito
    class MedicamentoPrescrito {
        -medicamento: Medicamento
        -dosis: String
        -frecuencia: String
        -viaAdministracion: String
        -duracion: String
        +getDosis(): String
        +getFrecuencia(): String
    }

    %% Clase Medicamento
    class Medicamento {
        -idMedicamento: String
        -nombre: String
        -nombreGenerico: String
        -laboratorio: String
        -stock: int
        -precio: double
        -fechaVencimiento: Date
        -requiereReceta: boolean
        +verificarStock(): boolean
        +actualizarStock(cantidad: int)
        +estaVencido(): boolean
    }

    %% Clase Farmacia
    class Farmacia {
        -nombreFarmacia: String
        -ubicacion: String
        -inventario: List~Medicamento~
        -farmaceutico: Usuario
        +agregarMedicamento(medicamento: Medicamento)
        +dispensarPrescripcion(prescripcion: Prescripcion): boolean
        +verificarStock(medicamento: Medicamento): int
        +generarOrdenCompra(): OrdenCompra
        +registrarVenta(venta: Venta)
    }

    %% Clase Examen
    class Examen {
        -idExamen: String
        -tipo: String
        -fecha: Date
        -resultado: String
        -estadoExamen: EstadoExamen
        -laboratorista: Usuario
        -paciente: Paciente
        -doctor: Doctor
        +realizarExamen()
        +registrarResultado(resultado: String)
        +enviarResultados()
    }



    %% Clase Cirugia
    class Cirugia {
        -idCirugia: String
        -tipoCirugia: String
        -fecha: DateTime
        -duracionEstimada: int
        -quirofano: String
        -cirujano: Doctor
        -asistentes: List~Doctor~
        -enfermeras: List~Enfermera~
        -anestesiologo: Doctor
        -estado: EstadoCirugia
        +programarCirugia()
        +cancelarCirugia()
        +registrarResultado()
    }



    %% Clase Signos Vitales
    class SignosVitales {
        -fecha: DateTime
        -presionArterial: String
        -frecuenciaCardiaca: int
        -temperatura: double
        -saturacionOxigeno: int
        -frecuenciaRespiratoria: int
        -peso: double
        -altura: double
        +registrarSignos()
        +esNormal(): boolean
    }

    %% Clase Horario
    class Horario {
        -diaSemana: String
        -horaInicio: Time
        -horaFin: Time
        -disponible: boolean
        +estaDisponible(): boolean
        +marcarOcupado()
    }

    %% Clase Habitacion
    class Habitacion {
        -numeroHabitacion: String
        -tipo: TipoHabitacion
        -piso: int
        -ocupada: boolean
        -pacienteActual: Paciente
        -precioDiario: double
        +asignarPaciente(paciente: Paciente)
        +liberarHabitacion()
        +estaDisponible(): boolean
    }



    %% Clase Factura
    class Factura {
        -idFactura: String
        -paciente: Paciente
        -fecha: Date
        -conceptos: List~ConceptoFactura~
        -subtotal: double
        -impuestos: double
        -total: double
        -estadoPago: EstadoPago
        +agregarConcepto(concepto: ConceptoFactura)
        +calcularTotal(): double
        +generarPDF(): File
        +registrarPago(monto: double)
    }



    %% Clase Concepto Factura
    class ConceptoFactura {
        -descripcion: String
        -cantidad: int
        -precioUnitario: double
        -subtotal: double
        +calcularSubtotal(): double
    }

    %% Clase Report
    class Report {
        -idReporte: String
        -titulo: String
        -fecha: Date
        -contenido: String
        -tipo: String
        -generadoPor: Usuario
        +generarPDF(): File
        +generarExcel(): File
        +enviarPorEmail(destinatario: String)
        +imprimir()
    }

    %% Relaciones
    Hospital "1" --> "*" Paciente: gestiona
    Hospital "1" --> "*" Doctor: emplea
    Hospital "1" --> "*" Departamento: tiene
    Hospital "1" --> "*" Farmacia: contiene
    Hospital "1" --> "1" GestorArchivos: utiliza
    
    Usuario <|-- Paciente: hereda
    Usuario <|-- Doctor: hereda
    Usuario <|-- Enfermera: hereda
    Usuario <|-- Administrador: hereda
    
    GestorArchivos --> Usuario: gestiona
    
    Paciente "1" --> "*" CitaMedica: solicita
    Paciente "1" --> "1" ExpedienteMedico: tiene
    Paciente --> EstadoPaciente: tiene
    Paciente "*" --> "1" Doctor: asignado a
    Paciente "1" --> "*" Factura: recibe
    
    Doctor "1" --> "*" CitaMedica: atiende
    Doctor "*" --> "1" Departamento: pertenece
    Doctor "1" --> "*" Horario: tiene
    Doctor "1" --> "*" Prescripcion: emite
    Doctor "1" --> "*" Diagnostico: realiza
    Doctor "1" --> "*" Cirugia: realiza
    
    Enfermera --> SignosVitales: registra
    Enfermera "*" --> "*" Cirugia: asiste
    
    CitaMedica --> EstadoCita: tiene
    CitaMedica "1" --> "1" Doctor: con
    CitaMedica "1" --> "1" Paciente: para
    
    ExpedienteMedico "1" --> "*" Diagnostico: contiene
    ExpedienteMedico "1" --> "*" Prescripcion: contiene
    ExpedienteMedico "1" --> "*" Examen: incluye
    ExpedienteMedico "1" --> "*" Cirugia: registra
    
    Prescripcion "1" --> "*" MedicamentoPrescrito: contiene
    MedicamentoPrescrito "*" --> "1" Medicamento: referencia
    
    Farmacia "1" --> "*" Medicamento: almacena
    Farmacia --> Prescripcion: dispensa
    
    Examen --> EstadoExamen: tiene
    Examen "*" --> "1" Paciente: para
    Examen "*" --> "1" Doctor: solicitado por
    
    Cirugia --> EstadoCirugia: tiene
    Cirugia "*" --> "1" Paciente: para
    
    Departamento "1" --> "*" Usuario: tiene
    
    Habitacion --> TipoHabitacion: es tipo
    Habitacion "0..1" --> "1" Paciente: ocupa
    
    Factura "1" --> "*" ConceptoFactura: contiene
    Factura --> EstadoPago: tiene
    Factura "*" --> "1" Paciente: para
    
    Hospital "1" --> "*" Report: genera
    Administrador "1" --> "*" Report: crea
    Departamento "1" --> "*" Report: produce
