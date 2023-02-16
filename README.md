# swiftui_extension_swipe_dismissable

```swift
struct ModalView: View {
    
    @GestureState var draggingOffset: CGSize = .zero
    @State var backgroundOpacity: CGFloat = 1
    @Environment(\.dismiss) var dismiss: DismissAction
    
    var body: some View {
        ZStack {
            Color
                .black
                .ignoresSafeArea()
                .opacity(self.backgroundOpacity)
        }
        .bottomDragDismissable(backgroundOpacity: self.$backgroundOpacity,
                               offset: self.$draggingOffset,
                               onDismiss: {
            self.dismiss()
        })
//        .leftDragDismissable(backgroundOpacity: self.$backgroundOpacity,
//                             offset: self.$draggingOffset,
//                             onDismiss: { self.dismiss() })
        
    }
    

    
}
```

```swift

extension View {
    
    func leftDragDismissable(backgroundOpacity: Binding<CGFloat>,
                             offset: GestureState<CGSize>,
                             dismissAt: CGFloat = UIScreen.main.bounds.width / 2,
                             onDismiss: (() -> Void)? = nil) -> some View {
        func onChange(value: CGSize) {
            // calculating opactiy...
            let progress: CGFloat = offset.wrappedValue.width / UIScreen.main.bounds.width

            withAnimation(.default) {
                DispatchQueue.main.async {
                    backgroundOpacity.wrappedValue = Double(1 - progress)
                }
            }

        }
        
        func onEnd(value: DragGesture.Value) {
            withAnimation(.easeInOut) {
                let translation: CGFloat = value.translation.width
                if translation > dismissAt {
                    onDismiss?()
                    backgroundOpacity.wrappedValue = 1
                    backgroundOpacity.wrappedValue = 1
                }
            }
        }
        
        return self
            .offset(x: offset.wrappedValue.width > 0 ? offset.wrappedValue.width : 0)
            .gesture(
                DragGesture()
                    .updating(offset, body: { value, outValue, _ in
                        outValue = value.translation
                        onChange(value: offset.wrappedValue)
                    })
                    .onEnded({ value in
                        onEnd(value: value)
                    })
            )
        
    }
    
    func bottomDragDismissable(backgroundOpacity: Binding<CGFloat>,
                               offset: GestureState<CGSize>,
                               dismissAt: CGFloat = UIScreen.main.bounds.height / 2,
                               onDismiss: (() -> Void)? = nil) -> some View {
        
        func onChange(value: CGSize) {
            // calculating opactiy...
            let progress: CGFloat = offset.wrappedValue.height / UIScreen.main.bounds.height

            withAnimation(.default) {
                DispatchQueue.main.async {
                    backgroundOpacity.wrappedValue = Double(1 - progress)
                }
            }

        }
        
        func onEnd(value: DragGesture.Value) {
            withAnimation(.easeInOut) {
                let translation: CGFloat = value.translation.height
                if translation > dismissAt {
                    onDismiss?()
                    backgroundOpacity.wrappedValue = 1
                    backgroundOpacity.wrappedValue = 1
                }
            }
        }
        
        return self
            .offset(y: offset.wrappedValue.height > 0 ? offset.wrappedValue.height : 0)
            .gesture(
                DragGesture()
                    .updating(offset, body: { value, outValue, _ in
                        outValue = value.translation
                        onChange(value: offset.wrappedValue)
                    })
                    .onEnded({ value in
                        onEnd(value: value)
                    })
            )
        

    }
    

    
}


```
