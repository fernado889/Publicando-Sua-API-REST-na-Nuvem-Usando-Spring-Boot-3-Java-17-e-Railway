# Publicando-Sua-API-REST-na-Nuvem-Usando-Spring-Boot-3-Java-17-e-Railway
// Activity.java (entidade)

@Entity

public class Activity {

  

 @Id

 @GeneratedValue(strategy = GenerationType.IDENTITY)

 private Long id;

  

 private String name;

 private String description;

 private Date date;

 private int duration; // em minutos

 private int caloriesBurned; // em calorias

  

 // getters e setters

}



// ActivityRepository.java (repositório)

public interface ActivityRepository extends JpaRepository<Activity, Long> {

  

}



// ActivityService.java (serviço)

@Service

public class ActivityService {

  

 @Autowired

 private ActivityRepository activityRepository;

  

 public List<Activity> getAllActivities() {

  return activityRepository.findAll();

 }

  

 public Activity getActivity(Long id) {

  return activityRepository.findById(id).orElseThrow();

 }

  

 public Activity createActivity(Activity activity) {

  return activityRepository.save(activity);

 }

  

 public Activity updateActivity(Long id, Activity activity) {

  Activity existingActivity = getActivity(id);

  existingActivity.setName(activity.getName());

  existingActivity.setDescription(activity.getDescription());

  existingActivity.setDate(activity.getDate());

  existingActivity.setDuration(activity.getDuration());

  existingActivity.setCaloriesBurned(activity.getCaloriesBurned());

  return activityRepository.save(existingActivity);

 }

  

 public void deleteActivity(Long id) {

  activityRepository.deleteById(id);

 }

}



// ActivityController.java (controlador)

@RestController

@RequestMapping("/api/activities")

public class ActivityController {

  

 @Autowired

 private ActivityService activityService;

  

 @GetMapping

 public List<Activity> getAllActivities() {

  return activityService.getAllActivities();

 }

  

 @GetMapping("/{id}")

 public Activity getActivity(@PathVariable Long id) {

  return activityService.getActivity(id);

 }

  

 @PostMapping

 public Activity createActivity(@RequestBody Activity activity) {

  return activityService.createActivity(activity);

 }

  

 @PutMapping("/{id}")

 public Activity updateActivity(@PathVariable Long id, @RequestBody Activity activity) {

  return activityService.updateActivity(id, activity);

 }

  

 @DeleteMapping("/{id}")

 public void deleteActivity(@PathVariable Long id) {

  activityService.deleteActivity(id);

 }

}
