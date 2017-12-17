
When making the stupid game **Happy Mrs. Chiken**, I compared several techniques of playing AudioSource and tried to make myself satisfied.

## Goal
Every chicken will produce 2 fart sounds when clicked. If it is a bonus click, produce type 2 fart, else, produce type 1.

## Implementation
- In the first version, I just use the most simple method, which is directly write in a controller script appended to the chicken Prefab. The result is, codes belong to different parts of control appears interweavingly. So the whole script is very messy.
- Afterwards I realize the StateMachineBehaviour technuque. It can be appended to a state in Animator and executed after state transfer. Then I reconstructed the controller into this pattern.
- But in this pattern, the script needs to instantiate and destroy the audio object every time, which seems very meaningless and wasting. I panned to use Event System in the future and control the sound using *SendMessage()*.

## GameObject Script
Very Naïve and traditional version. 
- Add 2 AudioSource Components to every chicken
- Add a controller script to every chicken with a public AudioSource Array. And drag two AudioSource Components into the array
- Then everytime when a chicken's state is updated to *egging*, it will enter *Egg()* function and play the audio source

```
public class ChikenController : MonoBehaviour {
	... 
	// fart-related member variables
	public AudioSource[] fart;
	private bool bonus;
	...

	void Update () {
		if (...) {
			...
		} else if (state == ChikenState.egging) {
			Egg ();
		} ...
	}

	void Egg () {
		...
		int fartIndex = bonus ? 0 : 1;
		fart [fartIndex].Play ();
	}
	...
}

```

## State Machine Behaviour

In Animator, we can add a Behaviour script to every state. When transfer to certain state, its script will be executed. The execution time can be set manually by overwritting certain functions in [Unity StateMachineBehaviour documentation](https://docs.unity3d.com/ScriptReference/StateMachineBehaviour.html)

Since in Unity, AudioSource cannot be independent from a GameObject. So we need to create a host GameObject for every AudioSource and make it a Prefab.
![](/img/in-post/2017-12-16-unity-state-machine-audio/fart-inspector.png)

Then for the state which needs to produce sound, click Add Behaviour to create StateMachineBehaviour script. Here I need the fart script being executed when a chicken enter *Lay* state.
![](/img/in-post/2017-12-16-unity-state-machine-audio/add-behaviour.png)

**Caution**, AudioSource is referenced indirectly from a GameObject, which is not existed at the start of the script. Hence, we cannot directly claim Component like before. We **must instantiate** the GameObject and then get Component from the instance. Otherwise Unity will report a **Cannot play inactive sound** error. Of course, do not forget to destroy the instance when leave the state.

```
public class LayBehaviour : StateMachineBehaviour {
	public GameObject smallFartAudioObj;
	public GameObject bigFartAudioObj;

	...
	private bool bonus;
	private GameObject fartAudioObjInstance;

	override public void OnStateEnter(Animator animator,
	 AnimatorStateInfo stateInfo, 
	 int layerIndex) {
		...
		if (bonus == false) {
			// Important
			fartAudioObjInstance = Instantiate (smallFartAudioObj);
		} else {
			fartAudioObjInstance = Instantiate (bigFartAudioObj);
		}
		AudioSource fart = fartAudioObjInstance.GetComponent<AudioSource>();
		fart.Play ();
	}

	override public void OnStateExit(Animator animator,
	 AnimatorStateInfo stateInfo, 
	 int layerIndex) {
		Destroy (fartAudioObjInstance);
	}
}

```

## Event System Management
To be continued.