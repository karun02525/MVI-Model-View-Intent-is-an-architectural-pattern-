# MVI-Model-View-Intent-is-an-architectural-pattern

    MVI (Model-View-Intent) is an architectural pattern that has gained popularity in the Android development community. It provides a clear separation of concerns by dividing the components of an application into three main parts: Model, View, and Intent. MVI is often used with reactive programming principles and frameworks like RxJava or Kotlin Flow.

    The Intent component represents user interactions or events that occur in the UI. It captures the user's intentions and converts them into a format that can be processed by the Model.
    The Intent sends the user's actions to the Model, triggering state updates
    

#### MainActivity
   class MainActivity : AppCompatActivity() {
    
    private lateinit var viewModel: AuthViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        viewModel = ViewModelProvider(this).get(AuthViewModel::class.java)

        btnLogin.setOnClickListener {
            clickLogin()
        }
        btnLogout.setOnClickListener {
            logout()
        }

    }

    fun clickLogin(){
        viewModel.processIntent(AuthIntent.Login("karun","kaju"))
    }

    fun logout(){
        viewModel.processIntent(AuthIntent.Logout)
    }
}

#### AuthViewModel
    class AuthViewModel @Inject constructor(private val api:AuthUsecase) : ViewModel() {
        fun processIntent(intent: AuthIntent){
            when(intent){
                is AuthIntent.Login -> loginHandle(intent.username,intent.password)
                is AuthIntent.Logout -> logoutHandel()
            }
        }
        private fun logoutHandel() {
            api.logout()
        }
        private fun loginHandle(username: String, password: String) {
            api.login(username,password)
        }
    }
#### AuthIntent
sealed class AuthIntent {
    data class Login(val username:String,val password:String):AuthIntent()
    object Logout : AuthIntent()
}
