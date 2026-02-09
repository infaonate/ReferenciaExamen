# REFERENCIA RÁPIDA - ANDROID
## Room + Fragment + RecyclerView + OSMDroid

---

## ROOM - ENTITY
```java
@Entity(tableName = "nombre_tabla")
public class MiEntidad {

    @PrimaryKey(autoGenerate = true)
    private int id;

    private String campo;

    // Constructor, getters y setters
}
```

---

## ROOM - DAO
```java
@Dao
public interface MiDao {

    @Insert
    void insertar(MiEntidad entidad);

    @Query("SELECT * FROM nombre_tabla")
    List<MiEntidad> obtenerTodos();

    @Delete
    void eliminar(MiEntidad entidad);
}
```

---

## ROOM - DATABASE
```java
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
```

---

## FRAGMENT - ESTRUCTURA BÁSICA
```java
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
```

---

## RECYCLERVIEW - ADAPTER
```java
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
```

---

## RECYCLERVIEW - ONCLICK EN VIEWHOLDER
```java
// Dentro del ViewHolder
public void onClick(View v) {
    int posicion = getAbsoluteAdapterPosition();
    if (posicion != RecyclerView.NO_POSITION && listener != null) {
        listener.onItemClick(datos.get(posicion));
    }
}
```

---

## NAVEGACIÓN FRAGMENT ↔ ACTIVITY
```java
// Llamar a método de Activity desde Fragment
((MainActivity) getActivity()).metodoActivity();

// Reemplazar Fragment desde Activity
getSupportFragmentManager()
    .beginTransaction()
    .replace(R.id.contenedor, new MiFragment())
    .addToBackStack(null)
    .commit();
```

---

## ALERTDIALOG
```java
new AlertDialog.Builder(context)
    .setTitle("Título")
    .setMessage("Mensaje")
    .setPositiveButton("Sí", (dialog, which) -> {
        // Acción positiva
    })
    .setNegativeButton("No", null)
    .show();
```

---

## OSMDROID - MAPA BÁSICO
```java
// En onCreate o onCreateView
Configuration.getInstance().load(context, 
    PreferenceManager.getDefaultSharedPreferences(context));

MapView mapView = view.findViewById(R.id.mapView);
mapView.setMultiTouchControls(true);

// Centrar el mapa
IMapController mapController = mapView.getController();
mapController.setZoom(15.0);
GeoPoint startPoint = new GeoPoint(43.2630, -2.9350);  // Bilbao
mapController.setCenter(startPoint);
```

---

## OSMDROID - MARCADORES
```java
// Añadir un marcador
Marker marker = new Marker(mapView);
marker.setPosition(new GeoPoint(latitud, longitud));
marker.setTitle("Título del marcador");
marker.setAnchor(Marker.ANCHOR_CENTER, Marker.ANCHOR_BOTTOM);
mapView.getOverlays().add(marker);
mapView.invalidate();

// Listener de click en el marcador
marker.setOnMarkerClickListener((m, mapView) -> {
    // Acción al hacer click
    return true;
});
```

---

## RADIOGROUP
```java
RadioGroup radioGroup = view.findViewById(R.id.miRadioGroup);
int seleccionadoId = radioGroup.getCheckedRadioButtonId();

if (seleccionadoId == -1) {
    // No hay nada seleccionado
}

if (seleccionadoId == R.id.opcion1) {
    // Opción 1 seleccionada
}
```

---

## FORMATO DE NÚMEROS Y FECHAS
```java
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
```

---

**FIN DE REFERENCIA**
