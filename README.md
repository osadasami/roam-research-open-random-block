# Roam Research open random block

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/iY1gPBmWELc/0.jpg)](https://www.youtube.com/watch?v=iY1gPBmWELc)

```clojure
(defn get-random-block-id []
  (let
    [blocks (js/roamAlphaAPI.q "[:find ?buid :where [?e :block/uid ?buid]]")
     blocks-count (count blocks)
     rand-index (rand-int blocks-count)
     rand-block-id (first (nth blocks rand-index))]
    rand-block-id))

(defn prepare-url []
  (let
    [href (. js/location -href)]
    (if (clojure.string/includes? href "page")
      (-> href
        (clojure.string/replace #"page/.*" (str "page/" (get-random-block-id))))
      (-> href
        (str "/page/" (get-random-block-id)))
    )))

(defn focus-block [url]
  (set! (.. js/window -location) url))

(defn random-block [_]
  [:button
   {:style {:background-color "#000" :color "#fff"}
    :on-click (fn [] (
                (fn []
                  (js/console.log (prepare-url))
                  (focus-block (prepare-url))
                )
              ))}
   "Open random block"]
  )

```
