# ReferenciaExamen

Aquí tienes la referencia que tendrás disponible durante el examen


                        REFERENCIA RÁPIDA - ANDROID
                        Room + Fragment + RecyclerView + OSMDroid

--------------------------------------------------------------------------------
ROOM - ENTITY
--------------------------------------------------------------------------------

@Entity(tableName = "nombre_tabla")
public class MiEntidad {

    @PrimaryKey(autoGenerate = true)
    private int id;

    private String campo;

    // Constructor, getters y setters
}


--------------------------------------------------------------------------------
ROOM - DAO
--------------------------------------------------------------------------------

@Dao
public interface MiDao {

    @Insert
    void insertar(MiEntidad entidad);

    @Query("SELECT * FROM nombre_tabla")
    List<MiEntidad> obtenerTodos();

    @Delete
    void eliminar(MiEntidad entidad);
}


--------------------------------------------------------------------------------
ROOM - DATABASE
--------------------------------------------------------------------------------

@Database(entities = {MiEntidad.class}, version = 1)
public abstract class MiDatabase extends RoomDatabase {

    public abstract MiDao miDao();

    private static MiDatabase instance;

    public static MiDatabase getInstance(Context context) {
        if (instance == null) {
            instance = Room.databaseBuilder(
                context.getApplicationContext(),
                MiDatabase.class,
                "nombre_base_datos"
            ).allowMainThreadQueries().build();
        }
        return instance;
    }
}


--------------------------------------------------------------------------------
FRAGMENT - ESTRUCTURA BÁSICA
--------------------------------------------------------------------------------

public class MiFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater,
                             ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.mi_fragment, container, false);

        // Inicializar vistas y lógica aquí

        return view;
    }
}


--------------------------------------------------------------------------------
RECYCLERVIEW - ADAPTER
--------------------------------------------------------------------------------

public class MiAdapter extends RecyclerView.Adapter<MiAdapter.MiViewHolder> {

    private List<MiEntidad> datos;

    static class MiViewHolder extends RecyclerView.ViewHolder {
        TextView textView;
        Button boton;

        MiViewHolder(View itemView) {
            super(itemView);
            textView = itemView.findViewById(R.id.miTexto);
            boton = itemView.findViewById(R.id.miBoton);
        }
    }

    @Override
    public MiViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
            .inflate(R.layout.item_layout, parent, false);
        return new MiViewHolder(view);
    }

    @Override
    public void onBindViewHolder(MiViewHolder holder, int position) {
        MiEntidad item = datos.get(position);
        // Asignar datos a las vistas del holder
    }

    @Override
    public int getItemCount() {
        return datos.size();
    }

    public void actualizar(List<MiEntidad> nuevosDatos) {
        this.datos = nuevosDatos;
        notifyDataSetChanged();
    }
}


--------------------------------------------------------------------------------
OSMDROID - MAPA BÁSICO
--------------------------------------------------------------------------------

// En onCreate o onCreateView
Configuration.getInstance().load(context, 
    PreferenceManager.getDefaultSharedPreferences(context));


--------------------------------------------------------------------------------
OSMDROID - MARCADORES
--------------------------------------------------------------------------------

// Añadir un marcador
Marker marker = new Marker(mapView);

//Mostrar un marcador
mapView.getOverlays().add(marker);
//Refrescar el mapa
mapView.invalidate();



--------------------------------------------------------------------------------
RADIOGROUP
--------------------------------------------------------------------------------

RadioGroup radioGroup = view.findViewById(R.id.miRadioGroup);
int seleccionadoId = radioGroup.getCheckedRadioButtonId();

if (seleccionadoId == -1) {
    // No hay nada seleccionado
}

if (seleccionadoId == R.id.opcion1) {
    // Opción 1 seleccionada
}


--------------------------------------------------------------------------------
FORMATO DE NÚMEROS Y FECHAS
--------------------------------------------------------------------------------

// Formatear números con decimales
DecimalFormat formato = new DecimalFormat("#,##0.00");
String importeFormateado = formato.format(25.5);  // "25,50"

// Formatear fechas
SimpleDateFormat formatoFecha = new SimpleDateFormat("dd/MM/yyyy HH:mm",
                                                      Locale.getDefault());
Date fecha = new Date(timestamp);
String fechaFormateada = formatoFecha.format(fecha);

// Obtener timestamp actual
long ahora = System.currentTimeMillis();

---
                               FIN DE REFERENCIA
