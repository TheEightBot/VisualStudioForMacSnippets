## ReactiveUI / Xamarin Snippets

Add these snippets to **~/Library/VisualStudio/7.0/Snippets**

### wav 

```
this.WhenAnyValue(x => x.ViewModel.$Property$)
    .Subscribe($local$ => { })
    .DisposeWith($Bindings$);
```
 
### wao  

```
this.WhenAnyObservable(x => x.ViewModel.$SomeCommand$)
    .ObserveOn(RxApp.MainThreadScheduler)
    .Subscribe(val => { 
         System.Diagnostics.Debug.WriteLine("{0}", val);
    })
    .DisposeWith($ControlBindings$);
```

### bindc

 ```
this.BindCommand(ViewModel, x => x.$Command$, c => c._$control$)
    .DisposeWith(ControlBindings);
```

### bindone

```
this.OneWayBind(ViewModel, x => x.$Property$, c => c._$controlName$)
    .DisposeWith(ControlBindings);
```

### bindvm

```
this.Bind(ViewModel, x => x.$Property$, c => c._$controlName$)
    .DisposeWith(ControlBindings);
```

### bindpicker

```
this._$myPicker$.Picker
    .Bind(
        this.WhenAnyValue(x => x.ViewModel.$Collection$),
            x => ViewModel.$SomeId$ = x.$SomeId2$,
            x => ViewModel.$SomeId$ == x.$SomeId2$,
            x => x.$SomeLabel$)
    .DisposeWith(ControlBindings);
```

### bindlist

```
this.$_myList$.Bind(this.WhenAnyValue(x => x.ViewModel.Data))
    .DisposeWith(ControlBindings);

this.$_myList$
    .ListViewItemTapped
    .Subscribe(selected =>
    {
        $_myList$.SelectedItem = null;
    })
    .DisposeWith(ControlBindings);
```

### combine

```
Observable
    .CombineLatest(
        this.WhenAnyValue(x => x.ViewModel.$Prop1$),
        this.WhenAnyValue(x => x.ViewModel.$Prop2$), 
            ($val1$, $val2$) => string.Format("{0}, {1}", $val1$, $val2$))
    .BindTo(_$control$, c => c.Text)
    .DisposeWith(ControlBindings);
```


### merge

```
Observable
    .Merge(
        this.WhenAnyObservable(x => x.ViewModel.NavigateTo),
        this.WhenAnyObservable(x => x.ViewModel.NavigateTo))
    .Subscribe(val =>
    {

    }) 
    .DisposeWith($Bindings$);
```

### rc

```
  ReactiveCommand<Unit> _$myCommand$;
  [DataMember]
  public ReactiveCommand<Unit> $MyCommand$
  {
      get { return _$myCommand$; }
      private set
      {
          this.RaiseAndSetIfChanged(ref _$myCommand$, value);
      }
  }
```

### rcmd

```
$CommandName$ =
    ReactiveCommand
        .CreateAsyncTask(_ =>
        { 
            return Task.FromResult(Unit.Default);
        })
        .DisposeWith(ViewModelBindings);
```

### rtrig

```$SomeCommand$ = ReactiveCommand.Create().DisposeWith(ViewModelBindings);```

### propbind

```
public static BindableProperty $Some$Property =
    BindableProperty.Create(nameof($Some$), typeof($SomeType$), typeof($SomeClass$), default($SomeType$),
                            propertyChanged: (bindable, oldValue, newValue) => (bindable as $SomeClass$)?.InvalidateSurface());

  public $SomeType$ $Some$
  {
      get { return ($SomeType$)GetValue($Some$Property); }
      set { SetValue($Some$Property, value); }
  } 
```

### classvm

```
public class $Name$ : ViewModelBase<$Name$> 
{
//using System;
//using System.Reactive;
//using System.Reactive.Linq;
//using System.Runtime.Serialization;
//using System.Threading.Tasks;
//using EightBot.BigBang.Extensions;
//using EightBot.BigBang.ViewModel;
//using FluentValidation;
//using ReactiveUI;
//using Splat;

    public override string Title
    {
        get { return string.Empty; }
    }

    public override AbstractValidator<$Name$> Validator => null;
 
    public $Name$() {}

    protected override void Initialize()
    {
         base.Initialize();
    }

    ReactiveCommand<Unit> _initializeData;
    [DataMember]
    public ReactiveCommand<Unit> InitializeData
    {
        get { return _initializeData; }
        private set
        {
            this.RaiseAndSetIfChanged(ref _initializeData, value);
        }
    } 

    protected override void RegisterObservables()
    {
        InitializeData =
            ReactiveCommand
                .CreateAsyncTask(_ => {
                         
                    return System.Threading.Tasks.Task.FromResult(Unit.Default);
                })
                .DisposeWith(ViewModelBindings);

        /* Navigation Commands *****/
 

        /* Validation *****/


        /* Observables *****/
    }

}
```

### classui

```
public class $Name$ : ContentPageBase<ViewModels.$Name$>
{ 
//using System;
//using System.Reactive.Linq;
//using EightBot.BigBang.Extensions;
//using EightBot.BigBang.XamForms.Extensions;
//using EightBot.BigBang.XamForms.Pages;
//using EightBot.BigBang.XamForms.Views;
//using ReactiveUI;
//using Xamarin.Forms;

    Grid _mainLayout;

    public $Name$()
    {
        ViewModel = new ViewModels.$Name$();
    } 

    protected override void SetupUserInterface()
    {   
          _mainLayout = new Grid
          {
                BackgroundColor = Color.Transparent,
                HorizontalOptions = LayoutOptions.FillAndExpand,
                VerticalOptions = LayoutOptions.FillAndExpand,
                Margin = new Thickness(Values.Layout.Padding, Values.Layout.TriplePadding, Values.Layout.Padding, Values.Layout.Padding),
                ColumnSpacing = Values.Layout.Padding,
                RowSpacing = Values.Layout.HalfPadding,
                ColumnDefinitions = new ColumnDefinitionCollection { 
                    new ColumnDefinition { Width = GridLength.Star },
                },
                RowDefinitions = new RowDefinitionCollection {
                    new RowDefinition { Height = GridLength.Star },
                    new RowDefinition { Height = GridLength.Star }
                }
            };

        Content = _mainLayout;
    }

    protected override void BindControls()
    {  
        this.OneWayBind(ViewModel, x => x.Title, c => c.Title)
            .DisposeWith(ControlBindings);
 
        this.WhenAnyValue(x => x.ViewModel)
            .IsNotNull()
            .InvokeCommand(this, x => x.ViewModel.InitializeData)
            .DisposeWith(ControlBindings);
    }
}
```

### classcell

```
public class $Name$Cell : ReactiveViewCell<ViewModels.$Name$Item>
{ 
//using System;
//using System.Linq;
//using EightBot.BigBang.Extensions;
//using EightBot.BigBang.XamForms.Pages;
//using EightBot.BigBang.XamForms.Views;
//using ReactiveUI;
//using ReactiveUI.XamForms;
//using Xamarin.Forms;

    public const int RequestedHeight = Values.Layout.StandardCellHeight;

    Grid _mainLayout;

    public $Name$Cell()
    {
        ViewModel = new ViewModels.$Name$Item();
    } 

    protected override void SetupUserInterface()
    {   
         _mainLayout = new Grid
            {
                BackgroundColor = Color.Transparent,
                HorizontalOptions = LayoutOptions.FillAndExpand,
                VerticalOptions = LayoutOptions.FillAndExpand, 
                RowSpacing = Values.Layout.HalfPadding,
                ColumnDefinitions = new ColumnDefinitionCollection {
                        new ColumnDefinition { Width = GridLength.Star },
                    },
                RowDefinitions = new RowDefinitionCollection {
                        new RowDefinition { Height = GridLength.Star },
                        new RowDefinition { Height = GridLength.Star }
                    }
            };

            View = _mainLayout;
    }

    protected override void BindControls()
    {  
        this.WhenAnyValue(x => x.ViewModel)
            .IsNotNull()
            .InvokeCommand(this, x => x.ViewModel.InitializeData);
    }
}
```

### dw

```System.Diagnostics.Debug.WriteLine($"{val}");```

### layopt

```
HorizontalOptions = LayoutOptions.FillAndExpand,
VerticalOptions = LayoutOptions.FillAndExpand,
```

### locate

```var repo = Locator.CurrentMutable.GetService<$SomeService$>();```

### grid

```
_mainLayout = new Grid()
{
    ColumnDefinitions = new ColumnDefinitionCollection()
    {
        new ColumnDefinition() { Width = GridLength.Star }
    },
    RowDefinitions = new RowDefinitionCollection()
    {
        new RowDefinition() { Height = GridLength.Star },
    }
};
```

### label

```
_$myLabel$ = new Label
{ 
      Style = Values.Theme.LabelStyle,
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.CenterAndExpand
  };
```

### stopwatch

```
var stopWatch = new System.Diagnostics.Stopwatch();
stopWatch.Start();
stopWatch.Stop();
System.Diagnostics.Debug.WriteLine($"stopWatch time : {stopWatch.Elapsed}");
```
