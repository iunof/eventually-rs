# Usage
``` rust
struct ExampleState {name: String, password: [u8;1024]}

#[derive(Event)]
#[event(state = "ExampleState", 
        error = "ExampleError")]
enum ExampleEvent {
    Empty,
    WithNamedFields {a: i8, b: [i8;1024]},
    WithUnnamedFields(i8,String,Box<T>),
}
#[derive(Command)]
#[command(state = "ExampleState", 
            event = "ExampleEvent", 
            id = "ExampleId",
            error = "ExampleError")]
enum ExampleCommand {
    Command1,
    Command2(i8, u8),
    Command3{ info: usize, },
}

#[derive(Aggregate)]
#[aggregate(state = "ExampleState",
            error = "ExampleError", 
            id = "ExampleId", 
            command = "ExampleCommand", 
            error = "ExampleError")]
struct ExampleAggregate {};

#[agrimpl]
#[agrimpl(event = "ExampleEvent", command1 = "ExampleCommand")]
impl ExampleAggregate {
    fn apply_empty(mut state: ExampleState) -> Result<ExampleState, ExampleError> {
        Ok(state)
    }

    fn apply_with_named_fields(mut state: ExampleState, something: i8, another: [i8;1024]) -> Result<ExampleState, ExampleError> {
        state.password = another;
        Ok(state)
    }

    fn apply_with_named_fields(mut state: ExampleState, my_name: usize) -> Result<ExampleState, ExampleError> {
        state.name = format!{"{}", my_name.to_string()};
        Ok(state)
    }
        
    async fn handle_command1(&self, id: &ExampleId, state: &ExampleState) -> Result<Option<Vec<ExampleEvent>>, ExampleError> {
        Ok(None)
    }

    async fn handle_command2(&self, id: &ExampleId, state: &ExampleState, hm: i8, meh: u8) -> Result<Option<Vec<ExampleEvent>>, ExampleError> {
        todo!()
    }

    async fn handle_command2(&self, id: &ExampleId, state: &ExampleState, another: usize) -> Result<Option<Vec<ExampleEvent>>, ExampleError> {
        todo!()
    }
}
```

