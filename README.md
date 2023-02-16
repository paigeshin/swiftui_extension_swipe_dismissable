# swiftui_extension_swipe_dismissable

```swift
import SwiftUI

extension View {
    
    func leftDragDismissable(backgroundOpacity: Binding<CGFloat> = .constant(1),
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
                    backgroundOpacity.wrappedValue = 1
                    onDismiss?()
                } else {
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
    
    func bottomDragDismissable(backgroundOpacity: Binding<CGFloat> = .constant(1),
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
                    backgroundOpacity.wrappedValue = 1
                    onDismiss?()
                } else {
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
