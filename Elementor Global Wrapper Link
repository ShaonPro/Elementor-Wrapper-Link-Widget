// ===== Elementor Global Wrapper Link for ALL widgets =====

// Safety: only run if Elementor is active.
add_action('plugins_loaded', function () {
    if ( ! did_action('elementor/loaded') ) return;

    // Add controls to ALL widgets (the "common" element is a base for all).
    add_action('elementor/element/common/_section_style/after_section_end', function( $element ) {
        $element->start_controls_section(
            'sp_wrapper_link_section',
            [
                'label' => __('Wrapper Link', 'sp'),
                'tab'   => \Elementor\Controls_Manager::TAB_ADVANCED,
            ]
        );

        $element->add_control(
            'sp_wrapper_link_enable',
            [
                'label'        => __('Enable Wrapper Link', 'sp'),
                'type'         => \Elementor\Controls_Manager::SWITCHER,
                'return_value' => 'yes',
                'default'      => '',
            ]
        );

        $element->add_control(
            'sp_wrapper_link_url',
            [
                'label'       => __('Link', 'sp'),
                'type'        => \Elementor\Controls_Manager::URL,
                'dynamic'     => [ 'active' => true ],
                'placeholder' => 'https://example.com',
                'condition'   => [ 'sp_wrapper_link_enable' => 'yes' ],
            ]
        );

        $element->add_control(
            'sp_wrapper_link_mode',
            [
                'label'       => __('Mode', 'sp'),
                'type'        => \Elementor\Controls_Manager::SELECT,
                'default'     => 'js',
                'options'     => [
                    'js'      => __('Safe (JS – keep inner links clickable)', 'sp'),
                    'overlay' => __('Overlay (CSS – full cover; inner links blocked)', 'sp'),
                ],
                'condition'   => [ 'sp_wrapper_link_enable' => 'yes' ],
            ]
        );

        $element->end_controls_section();
    }, 10, 1);

    // Render filter: wraps widget output with data attributes or overlay anchor.
    add_filter('elementor/widget/render_content', function( $content, $widget ) {
        $settings = $widget->get_settings_for_display();

        if (
            empty($settings['sp_wrapper_link_enable']) ||
            'yes' !== $settings['sp_wrapper_link_enable'] ||
            empty($settings['sp_wrapper_link_url']['url'])
        ) {
            return $content;
        }

        $url        = $settings['sp_wrapper_link_url']['url'];
        $is_external= !empty($settings['sp_wrapper_link_url']['is_external']);
        $nofollow   = !empty($settings['sp_wrapper_link_url']['nofollow']);
        $mode       = !empty($settings['sp_wrapper_link_mode']) ? $settings['sp_wrapper_link_mode'] : 'js';

        // Build rel attribute safely.
        $rels = [];
        if ( $nofollow )   $rels[] = 'nofollow';
        if ( $is_external ) $rels[] = 'noopener';
        $rel_attr = $rels ? ' rel="'. esc_attr(implode(' ', array_unique($rels))) .'"' : '';

        if ( 'overlay' === $mode ) {
            // CSS overlay anchor covering entire widget (blocks inner links).
            $target_attr = $is_external ? ' target="_blank"' : '';
            $overlay  = '<a class="sp-wrapper-link-overlay" href="'. esc_url($url) .'"'. $target_attr . $rel_attr .' aria-label="Open link"></a>';
            $wrapped  = '<div class="sp-wrapper-link-box">'. $content . $overlay .'</div>';
            return $wrapped;
        }

        // Default "Safe (JS)" mode: add data attributes, let JS handle clicks (inner links still work).
        $target = $is_external ? '_blank' : '_self';
        $wrapped = '<div class="sp-wrapper-link-js" data-sp-wrapper-link="'. esc_url($url) .'" data-sp-wrapper-target="'. esc_attr($target) .'" data-sp-wrapper-rel="'. esc_attr(implode(' ', $rels)) .'">'
                 . $content
                 . '</div>';
        return $wrapped;
    }, 10, 2);

    // Enqueue tiny JS + CSS once on front-end.
    add_action('wp_enqueue_scripts', function () {
        // Inline CSS
        $css = '
        .sp-wrapper-link-box{position:relative;}
        .sp-wrapper-link-overlay{position:absolute; inset:0; z-index:10; text-indent:-9999px;}
        ';
        wp_register_style('sp-wrapper-link-style', false);
        wp_enqueue_style('sp-wrapper-link-style');
        wp_add_inline_style('sp-wrapper-link-style', $css);

        // Inline JS (delegated click handler for Safe/JS mode)
        $js = "
        (function(){
            document.addEventListener('click', function(e){
                var box = e.target.closest('.sp-wrapper-link-js');
                if(!box) return;

                // Ignore clicks on interactive elements inside the widget
                if (e.target.closest('a, button, input, select, textarea, [role=\"button\"], [role=\"link\"]')) return;

                var url = box.getAttribute('data-sp-wrapper-link');
                if(!url) return;

                var tgt = box.getAttribute('data-sp-wrapper-target') || '_self';
                // Respect cmd/ctrl-click to open new tab/window
                if (e.metaKey || e.ctrlKey) { window.open(url, '_blank'); return; }

                window.open(url, tgt);
            });

            // Keyboard accessibility: Enter key triggers wrapper link when box is focused
            document.addEventListener('keydown', function(e){
                if (e.key !== 'Enter') return;
                var el = document.activeElement;
                if (!el || !el.classList || !el.classList.contains('sp-wrapper-link-js')) return;

                var url = el.getAttribute('data-sp-wrapper-link');
                var tgt = el.getAttribute('data-sp-wrapper-target') || '_self';
                if (url) window.open(url, tgt);
            });

            // Make JS mode containers focusable for keyboard users
            window.addEventListener('load', function(){
                document.querySelectorAll('.sp-wrapper-link-js').forEach(function(el){
                    if (!el.hasAttribute('tabindex')) el.setAttribute('tabindex','0');
                    if (!el.hasAttribute('role')) el.setAttribute('role','link');
                });
            });
        })();
        ";
        wp_register_script('sp-wrapper-link-script', false);
        wp_enqueue_script('sp-wrapper-link-script');
        wp_add_inline_script('sp-wrapper-link-script', $js);
    });
});
